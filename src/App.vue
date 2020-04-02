<script>
  import Autocomplete from "@trevoreyre/autocomplete-vue";

  export default {
    name: "App",
    components: {
      Autocomplete
    },
    data() {
      return {
        data: {},
        selected: "",
        amount: "",
        showForm: false,
        model: [],
        email: "",
        formUrl: process.env.VUE_APP_FORM_URL,
        orderCreateUrl: process.env.VUE_APP_ORDER_URL,
        orderID: null
      };
    },
    mounted() {
      this.loadData();
    },
    computed: {
      getPrice() {
        if (this.selected.formula === "per_person") {
          return this.model.filter(item => item.name).length * this.selected.price;
        }
        return this.selected.price;
      },
      activateButton() {
        return this.model.filter(item => item.name).length
      },
      getSeparatedName() {
        const [fist, second] = this.data.fields.names.title.split(":");
        return {fist, second}
      },
      getSeparatedListTile() {
        const [fist, second] = this.selected.title.split("-");
        return {fist, second}
      },
    },
    methods: {
      loadData() {
        fetch(this.formUrl)
          .then((response) => {
            return response.json();
          })
          .then((myJson) => {
            this.data = myJson;
            this.selected = this.data.fields.service.values[0].items[0];
            this.model = [...new Array(this.data.fields.names.max)].map(() => ({
              name: "",
              notice: ""
            }));
            this.showForm = true;
          })
          .catch((e) => {
            alert(e);
          });
      },
      submit(e) {
        e.preventDefault();

        if (!this.activateButton) {
          return;
        }

        if (!this.$refs.payForm.reportValidity()) {
          return;
        }

        fetch(this.orderCreateUrl, {
          method: 'POST',
          mode: 'cors',
          body: JSON.stringify({
            offer_id: this.selected.id,
            amount: this.$refs.amount.value,
            email: this.email,
            names_list: this.model
          })
        })
          .then((response) => {
            return response.json();
          })
          .then((myJson) => {
            this.orderID = myJson.order_id;
            this.$nextTick(() => {
              this.$refs.payForm.submit();
            })
          })
          .catch((e) => {
            alert(e);
          });
      },
      getNotice(e, index) {
        this.model[index].notice = e.target.value;
      },
      liveSearch(input) {
        return input ? this.data.fields.names.marks.filter(item => item.includes(input)) : [];
      }
    }
  };
</script>


<template>
  <div id="app">
    <section v-if="showForm">
      <h1>{{data.title}}</h1>
      <form method="POST" action="https://money.yandex.ru/quickpay/confirm.xml" ref="payForm">
        <ul id="riteContainer">
          <li>
            <span class="spanRite">{{data.fields.service.title}}</span>
            <div class="popup">
              <button @click.prevent class="popupButton">?</button>
              <div class="popupContent">{{ data.fields.service.hint }}</div>
            </div>
          </li>
          <li>
            <select name="selectRites" v-model="selected" id="selectRites">
              <optgroup
                :key="index"
                v-for="(category, index) in data.fields.service.values"
                :label="category.title"
              >
                <option :key="index" v-for="(item, index) in category.items" :value="item">
                  {{item.title}}
                </option>
              </optgroup>
            </select>
          </li>
          <li>
            <span class="spanRite">{{ getSeparatedName.fist }}:</span>
            <span id="spanObjection">{{ getSeparatedName.second }}</span>
            <div class="popup">
              <button @click.prevent class="popupButton">?</button>
              <div class="popupContent">{{ data.fields.names.hint }}</div>
            </div>
          </li>
          <li>
            <div
              id="riteForHealth"
              :style="{'border' : '2px solid ' + selected.color, 'color' : selected.color}"
            >
              <img
                :src="require('./assets/corner_' + selected.color + '.png')"
                alt="corner"
                class="corner"
              >
              <img
                :src="require('./assets/corner_' + selected.color + '.png')"
                alt="corner"
                class="corner"
              >
              <div id="riteForHealthLogo">
                <img :src="require('./assets/rites_' + selected.color + '.png')" alt="Missing Crest"/>
                <div>{{ getSeparatedListTile.fist }}</div>
                <div>{{ getSeparatedListTile.second }}</div>
              </div>
              <ul id="listRiteForHealth">
                <li
                  :style="{'border-bottom' : '1px solid ' + selected.color}" :key="index"
                  v-for="(row, index) in data.fields.names.max"
                >
                  <span>{{row}}.</span>
                  <input
                    v-model="model[index].name"
                    type="text"
                    :name="'person_name' + index"
                    placeholder="Имя"
                  />
                  <Autocomplete
                    @input.native="getNotice($event, index)"
                    :search="liveSearch"
                    placeholder="Пометка"
                  ></Autocomplete>
                </li>
              </ul>
              <img
                :src="require('./assets/corner_' + selected.color + '.png')"
                alt="corner"
                class="corner"
              >
              <img
                :src="require('./assets/corner_' + selected.color + '.png')"
                alt="corner"
                class="corner"
              >
            </div>
          </li>
          <li>
            <div>
              <span class="spanRite">{{data.fields.donate.title}}:</span>
              <div class="popup">
                <button @click.prevent class="popupButton">?</button>
                <div class="popupContent">{{ data.fields.amount.hint }}</div>
              </div>
            </div>
            <input :value="getPrice" ref="amount" type="number" name="sum" id="inputSum"/>
          </li>
          <li>
            <div>
              <span class="spanRite">{{data.fields.email.title}}:</span>
              <div class="popup">
                <button @click.prevent class="popupButton">?</button>
                <div class="popupContent">{{ data.fields.email.hint }}</div>
              </div>
            </div>
            <input type="text" name="order_id" ref="order_id" v-model="orderID" v-show="true">
            <input type="email" required v-model="email" placeholder="для уведомлений" name="email" id="inputEmail"/>
          </li>
          <li>
            <span class="spanRite">{{data.fields.amount.title}}:</span>
            <div class="popup">
              <button @click.prevent class="popupButton">?</button>
              <div class="popupContent">{{ data.fields.amount.hint }}</div>
            </div>
          </li>
          <li>
            Метод оплаты:<br>
            <label><input type="radio" name="paymentType" value="PC">Яндекс.Деньгами</label><br>
            <label><input type="radio" name="paymentType" value="AC">Банковской картой</label><br>
            <label><input type="radio" name="paymentType" value="MC">C баланса мобильного. </label>

            <input type="hidden" name="receiver" value="41001303442475">
            <input type="hidden" name="formcomment" value="Заказ поминовений">
            <input type="hidden" name="short-dest" value="Заказ поминовений">
            <input type="hidden" name="quickpay-form" value="shop">
            <input type="hidden" name="targets" id="targets" :value="'Заказ ' + orderID" placeholder="description"><br>
            <input type="hidden" name="comment" value="Заказ поминовений">
            <input type="hidden" name="need-fio" value="false">
            <input type="hidden" name="need-email" value="false">
            <input type="hidden" name="need-phone" value="false">
            <input type="hidden" name="need-address" value="false">

          </li>
          <li>
            <button class="buttonSpare" :class="{buttonSpareDisabled: !activateButton}" ref="submitButton"
                    @click="submit">
              Пожертвовать
            </button>
          </li>
        </ul>
      </form>
    </section>
    <div class="spinner" v-if="!showForm"></div>
  </div>
</template>

<style lang="scss">


  .spinner {
    z-index: 10000;
    opacity: 1;
    width: 80px;
    height: 80px;

    border: 2px solid #f3f3f3;
    border-top: 3px solid #30365f;
    border-radius: 100%;

    position: absolute;
    top: 50%;
    left: 40%;

    animation: spin 1s infinite linear;
  }

  @keyframes spin {
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }


  $brown-color: #94683b;
  $bright-brown-color: #e2e2db;
  $dark-brown-color: #94683b;

  * {
    padding: 0;
    margin: 0;
    position: relative;
  }

  ul {
    list-style: none;
  }

  a {
    text-decoration: none;
  }

  .bold {
    font-weight: bold;
  }

  input {
    padding: 2px 0;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    max-width: 400px;
    height: 800px;
  }

  section {
    position: absolute;
    left: 0;
    top: 0;
    background: #e2e2db;
    margin: 0;

    h1 {
      font: 35px / 1 Georgia, serif, Arial;
    }

    #riteContainer {
      display: grid;
      grid-template-rows: auto;
    }

    .popup {
      display: inline-block;

      .popupButton {
        border: none;
        border-radius: 50%;
        background: $brown-color;
        padding: 0 2px;
        font-weight: bold;
        color: $bright-brown-color;
        font-size: 12px;

        &:hover {
          cursor: pointer;
        }

        &:hover + .popupContent {
          display: block
        }
      }

      .popupContent {
        background: #000;
        color: #fff;
        font-size: 14px;
        padding: 5px;
        border-radius: 5px;
        position: absolute;
        left: 0;
        top: 0;
        transform: translate(-45%, -100%);
        width: 356px;
        z-index: 3;
        display: none;
      }
    }

    .spanHint, .spanRite {
      color: $brown-color;
      font-weight: bold;
    }

    .spanRite {
      line-height: 2.5;
    }

    #spanObjection {
      font-weight: bold;
    }

    #riteForHealth {
      background: #fff;
    }

    .corner {
      background-size: cover;
      height: 30px;
      width: 30px;
      position: absolute;

      &:nth-child(1) {
        left: 0;
        top: 0;
      }

      &:nth-child(2) {
        right: 0;
        top: 0;
        transform: rotate(90deg);
      }

      &:nth-child(5) {
        left: 0;
        bottom: 0;
        transform: rotate(-90deg);
      }

      &:nth-child(6) {
        right: 0;
        bottom: 0;
        transform: rotate(180deg);
      }
    }

    #riteForHealthLogo {
      text-align: center;
      padding-bottom: 15px;

      img {
        height: 30px;
        padding: 15px 0 0 0;
      }

      div {
        font-family: "Ponomar Unicode";

        &:nth-child(2) {
          font-size: 30px;
          line-height: 1.5;
        }

        &:nth-child(3) {
          font-size: 16px;
        }
      }
    }

    #listRiteForHealth {
      li {
        display: grid;
        grid-template-columns: 1fr 5fr 5fr;
        width: 95%;
        padding: 5px 0;
        margin: auto;
        font-family: Georgia, serif, Arial;

        span {
          text-align: center;
        }

        input {
          border: none;
          width: 90%;
          font-size: 16px;
          font-style: italic;
        }

        &:last-child {
          margin-bottom: 50px;
        }

        ul {
          background: #fff;
          border: 1px solid #3d3d3d;
          border-radius: 5px;
          max-width: 90%;
          color: #3d3d3d;
          height: 100%;
          padding: 0 0 5px 0;
        }
      }
    }

    #listPaymentMethods {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-template-rows: auto;

      input {
        position: absolute;
        width: 100%;
        height: 100%;
        opacity: 0;
        z-index: 3;
      }

      img {
        width: 95%;
        background: #fff;
        opacity: 0.6;
        margin: 0 0 0 3px;
      }

      input:checked + img {
        opacity: 1;
        outline: 3px solid $dark-brown-color;
      }
    }

    .buttonSpare {
      border: 1px solid #3e8f3e;
      border-radius: 5px;
      padding: 10px 16px;
      background: linear-gradient(to bottom, #5cb85c 0, #419641 100%);
      color: #fff;
      font-family: Georgia, serif, Arial;
      font-size: 16px;
      font-weight: 500;
      cursor: pointer;
      margin: 15px 0;
    }

    .buttonSpareDisabled {
      border: 1px solid #618f63;
      border-radius: 5px;
      padding: 10px 16px;
      background: linear-gradient(to bottom, #76b870 0, #7c9678 100%);
      color: #cacaca;
      font-family: Georgia, serif, Arial;
      font-size: 16px;
      font-weight: 500;
      margin: 15px 0;
    }

    #helpMail {
      color: $brown-color;
      font-size: 16px;

      &:hover {
        text-decoration: underline;
      }
    }
  }
</style>
