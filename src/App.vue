<template>
  <div id="app">
    <section>
      <h1>Заказ поминовений</h1>
      <ul id="riteContainer">
        <li>
          <span class="spanRite">Поминовение </span>
          <span class="showPopup">?</span>
        </li>
        <li>
          <select name="selectRites" id="selectRites">
            <optgroup label="О здравии">
              <option value="riteHealth1">О здравии - Разовое поминовение (1 день)</option>
              <option value="riteHealth2">О здравии - Проскомидия, сорокоуст (40 дней)</option>
              <option value="riteHealth3">О здравии - Неусыпаемая Псалтирь (1 год)</option>
              <option value="riteHealth4">О здравии - Молебен пред иконой "Валаамская" (1 день)</option>
            </optgroup>
            <optgroup label="О упокоении">
              <option value="riteRest1">О упокоении - Разовое поминовение (1 день)</option>
              <option value="riteRest2">О упокоении - Проскомидия, сорокоуст (40 дней)</option>
              <option value="riteRest3">О упокоении - Проскомидия (1 год)</option>
              <option value="riteRest4">О упокоении - Неусыпаемая Псалтирь (1 год)</option>
              <option value="riteRest5">О упокоении - Панихида (1 день)</option>
            </optgroup>
          </select>
        </li>
        <li>
          <span class="spanRite">Имена:</span>
          <span id="spanObjection">в родительном падеже </span>
          <span></span>
          <span class="showPopup">?</span>
        </li>
        <li>
          <div id="riteForHealth">
            <div class="corner" id="cornerTopLeft"></div>
            <div class="corner" id="cornerTopRight"></div>
            <div id="riteForHealthLogo">
              <img src="../src/assets/rites.png" alt="Missing Crest" />
              <div>О здравии</div>
              <div>Разовое поминовение (1 день)</div>
            </div>
            <ul id="listRiteForHealth">
              <li>
                <span>1.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
              <li>
                <span>2.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
              <li>
                <span>3.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
              <li>
                <span>4.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
              <li>
                <span>5.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
              <li>
                <span>6.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
              <li>
                <span>7.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
              <li>
                <span>8.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
              <li>
                <span>9.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
              <li>
                <span>10.</span>
                <input type="text" placeholder="Имя" />
                <input type="text" placeholder="пометка" />
              </li>
            </ul>
            <div class="corner" id="cornerBottomLeft"></div>
            <div class="corner" id="cornerBottomRight"></div>
          </div>
        </li>
        <li>
          <div>
            <span class="spanRite">Сумма (рекомендуемая):</span>
            <span class="showPopup">?</span>
          </div>
          <input type="number" value="0" id="inputSum"/>
        </li>
        <li>
          <div>
            <span class="spanRite">Эллектроная почта:</span>
            <span class="showPopup">?</span>
          </div>
          <input type="email" placeholder="для уведомлений" id="inputEmail"/>
        </li>
        <li>
          <span class="spanRite">Способ пожертвования:</span>
          <span class="showPopup">?</span>
        </li>
        <li>
          <ul id="listPaymentMethods">
            <li>
              <input type="radio" name="payment" checked/>
              <img src="../src/assets/yandex.png" alt="yandex" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/visa.png" alt="VISA" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/sber.png" alt="sber" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/phone.png" alt="phone" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/tinikoff.png" alt="tinikoff" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/webmoney.png" alt="webmoney" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/russianMail.png" alt="russianMail" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/westernUnion.png" alt="westernUnion" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/forsaj.png" alt="forsaj" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/gPay.png" alt="gPay" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/bitcoin.png" alt="bitcoin" />
            </li>
            <li>
              <input type="radio" name="payment" />
              <img src="../src/assets/cash.png" alt="cash" />
            </li>
          </ul>
        </li>
        <li>
          <button id="buttonSpare">Пожертвовать</button>
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
  h1 {
    font: 35px / 1 Georgia, serif, Arial;
  }

  #riteContainer {
    display: grid;
    grid-template-rows: auto;
  }
  .spanRite {
    color: $brown-color;
    font-weight: bold;
    line-height: 2.5;
  }

  #spanObjection {
    font-weight: bold;
  }

  .showPopup {
    background: $brown-color;
    border-radius: 50%;
    padding: 0 3px;
    color: $bright-brown-color;
    font-weight: bold;
    font-size: 12px;
    &:hover {
      cursor: pointer;
    }
  }

  #riteForHealth {
    background: #fff;
    border: 2px solid red;
  }

  .corner {
    background: url(assets/corner.png);
    background-size: cover;
    height: 30px;
    width: 30px;
    position: absolute;
  }

  #cornerTopLeft {
    left: 0;
    top: 0;
  }

  #cornerTopRight {
    right: 0;
    top: 0;
    transform: rotate(90deg);
  }

  #cornerBottomLeft {
    left: 0;
    bottom: 0;
    transform: rotate(-90deg);
  }

  #cornerBottomRight {
    right: 0;
    bottom: 0;
    transform: rotate(180deg);
  }

  #riteForHealthLogo {
    text-align: center;
    padding-bottom: 15px;
    img {
      color: red;
      height: 30px;
      padding: 15px 0 0 0;
    }
    div {
      color: red;
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
      border-bottom: 1px solid red;
      width: 95%;
      padding: 5px 0;
      margin: auto;
      font-family: Georgia, serif, Arial;
      span {
        text-align: center;
        color: red;
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
      opacity: 0.8;
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
