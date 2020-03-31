<script>
    import Autocomplete from '@trevoreyre/autocomplete-vue'

    export default {
        name: "App",
        components: {
            Autocomplete
        },
        data() {
            return {
                data: {},
                selected: '',
                donate: '',
                showForm: false,
                model: []
            }
        },
        async mounted() {
            await this.loadData()
        },
       computed: {
            getPrice () {
                if (this.selected.formula === 'per_person') {
                    return this.model.filter(item => item.name).length * this.selected.price
                }
                return this.selected.price
            }
       },
        methods: {
            async loadData() {
                this.data = (await import('./assets/data')).default
                this.selected = this.data.fields.service.values[0].items[0]
                this.model = [...new Array(this.data.fields.names.max)].map(() => ({
                    name: "",
                    mark: ""
                }))

                this.showForm = true
            },
            submit () {
                console.log(this.$refs.donate.value)
            },
            showMark (e, index) {
                this.model[index].mark = e.target.value
            },
            search(input) {
                if (!input) {
                    return []
                }
                return this.data.fields.names.marks.filter(item => item.includes(input))
            }
        }
    }
</script>


<template>
  <div id="app">
    <section v-if="showForm">
      <h1>{{data.title}}</h1>
      <ul id="riteContainer">
        <li>
          <span class="spanRite">{{data.fields.service.title}}</span>
          <div class="popup">
            <button class="popupButton">?</button>
            <div class="popupContent">Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem
              Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem ipsum <span class="spanHint">{{ data.fields.service.hint }} »</span></div>
          </div>
        </li>
        <li>
          <select name="selectRites" v-model="selected" id="selectRites">
            <optgroup
              :key="index"
              v-for="(category, index) in data.fields.service.values"
              :label="category.title"
            >
              <option :key="index" v-for="(item, index) in category.items" :value="item">{{item.title}}
              </option>
            </optgroup>
          </select>
        </li>
        <li>
            <span class="spanRite">{{ data.fields.names.title.split(':')[0]}}:</span>
            <span id="spanObjection">{{ data.fields.names.title.split(':')[1]}}</span>
          <div class="popup">
            <button class="popupButton">?</button>
            <div class="popupContent">Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem
              Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem <span class="spanHint">{{ data.fields.names.hint }} »</span></div>
          </div>
        </li>
        <li>
          <div id="riteForHealth" :style="{'border' : '2px solid ' + selected.color, 'color' : selected.color}">
            <img :src="require('./assets/corner_' + selected.color + '.png')" alt="corner" class="corner">
            <img :src="require('./assets/corner_' + selected.color + '.png')" alt="corner" class="corner">
            <div id="riteForHealthLogo">
              <img :src="require('./assets/rites_' + selected.color + '.png')" alt="Missing Crest"/>
              <div>{{selected.title.split('-')[0]}}</div>
              <div>{{selected.title.split('-')[1]}}</div>
            </div>
            <ul id="listRiteForHealth">
              <li :style="{'border-bottom' : '1px solid ' + selected.color}" :key="index"
                  v-for="(row, index) in data.fields.names.max">
                <span>{{row}}.</span>
                <input v-model="model[index].name" type="text" placeholder="Имя"/>
                <Autocomplete @input.native="showMark($event, index)" :search="search" placeholder="Пометка"></Autocomplete>
              </li>
            </ul>
            <img :src="require('./assets/corner_' + selected.color + '.png')" alt="corner" class="corner">
            <img :src="require('./assets/corner_' + selected.color + '.png')" alt="corner" class="corner">
          </div>
        </li>
        <li>
          <div>
            <span class="spanRite">{{data.fields.amount.title}}:</span>
            <div class="popup">
              <button class="popupButton">?</button>
              <div class="popupContent">Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem
                Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem <span class="spanHint">{{ data.fields.amount.hint }} »</span></div>
            </div>
          </div>
          <input :value="getPrice" ref="donate" type="number" id="inputSum"/>
        </li>
        <li>
          <div>
            <span class="spanRite">{{data.fields.email.title}}:</span>
            <div class="popup">
              <button class="popupButton">?</button>
              <div class="popupContent">Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem
                Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem <span class="spanHint">{{ data.fields.email.hint }} »</span></div>
            </div>
          </div>
          <input type="email" placeholder="для уведомлений" id="inputEmail"/>
        </li>
        <li>
          <span class="spanRite">{{data.fields.donate.title}}:</span>
          <div class="popup">
            <button class="popupButton">?</button>
            <div class="popupContent">Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem
              Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem ipsum lorem ipsum lorem Lorem <span class="spanHint">{{ data.fields.donate.hint }} »</span></div>
          </div>
        </li>
        <li>
          <ul id="listPaymentMethods">
            <li>
              <input type="radio" name="payment" checked/>
              <img src="../src/assets/yandex.png" alt="yandex"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/visa.png" alt="VISA"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/sber.png" alt="sber"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/phone.png" alt="phone"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/tinikoff.png" alt="tinikoff"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/webmoney.png" alt="webmoney"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/russianMail.png" alt="russianMail"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/westernUnion.png" alt="westernUnion"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/forsaj.png" alt="forsaj"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/gPay.png" alt="gPay"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/bitcoin.png" alt="bitcoin"/>
            </li>
            <li>
              <input type="radio" name="payment"/>
              <img src="../src/assets/cash.png" alt="cash"/>
            </li>
          </ul>
        </li>
        <li>
          <button id="buttonSpare" @click="submit()">Пожертвовать</button>
        </li>
        <li>
          При возникновении вопросов пишите на
          <a
            href="mailto:rites@valaam.ru"
            target="_blank"
            id="helpMail"
          >rites@valaam.ru</a>.
        </li>
      </ul>
    </section>
  </div>
</template>

<style lang="scss">
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
  }

  section {
    position: absolute;
    left: 50%;
    top: 0;
    background: #e2e2db;
    max-width: 400px;
    transform: translate(-50%);
    margin: 200px 0;

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

    #buttonSpare {
      border: 1px solid #3e8f3e;
      border-radius: 5px;
      padding: 10px 16px;
      background: linear-gradient(to bottom, #5cb85c 0, #419641 100%);
      color: #fff;
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
