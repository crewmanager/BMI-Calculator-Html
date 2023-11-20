# BMI-Calculator-Html

`main.mo`

```js
actor {
  public func calculateBMI(weightInKg : Float, heightInCM : Float) : async (Float, Text) {
    let bmi = (weightInKg / heightInCM / heightInCM) * 10000;
    let category = if (bmi < 18.5) {
      "Underweight";
    } else if (bmi >= 18.5 and bmi < 24.9) {
      "Normal weight";
    } else if (bmi >= 24.9 and bmi < 29.9) {
      "Overweight";
    } else {
      "Obesity";
    };

    return (bmi, category);
  };

};

```
`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="main.css" />
    <title>BMI Calculator</title>
  </head>

  <body>
    <div class="container">
      <div class="box">
        <h1>BMI Calculator</h1>
        <div class="content">
          <div class="input">
            <label for="height">Age</label>
            <input
              type="number"
              class="text-input"
              id="age"
              autocomplete="off"
              required
              min="1"
              max="100"
            />
          </div>

          <div class="gender">
            <label class="container">
              <input type="radio" name="radio" id="m" />
              <p class="text">Male</p>
              <span class="checkmark"></span>
            </label>

            <label class="container">
              <input type="radio" name="radio" id="f" />
              <p class="text">Female</p>
              <span class="checkmark"></span>
            </label>
          </div>

          <div class="containerHW">
            <div class="inputH">
              <label for="height">Height(cm)</label>
              <input type="number" id="height" required />
            </div>

            <div class="inputW">
              <label for="weight">Weight(kg)</label>
              <input type="number" id="weight" required />
            </div>
          </div>
          <button class="calculate" id="submit">Calculate BMI</button>
        </div>
        <div class="result">
          <p>Your BMI is</p>
          <div id="result">00.00</div>
          <p class="comment"></p>
        </div>
        <div class="footer"></div>
      </div>
    </div>
    <!-- The Modal -->
    <div id="myModal" class="modal">
      <!-- Modal content -->
      <div class="modal-content">
        <span class="close">&times;</span>
        <p id="modalText"></p>
      </div>
    </div>
  </body>
</html>

```

`index.js`
```js
import { todo_backend } from "../../declarations/todo_backend";

var age = document.getElementById("age");
var height = document.getElementById("height");
var weight = document.getElementById("weight");
var male = document.getElementById("m");
var female = document.getElementById("f");
var form = document.getElementById("form");
let resultArea = document.querySelector(".comment");

var modalContent = document.querySelector(".modal-content");
var modalText = document.querySelector("#modalText");
var modal = document.getElementById("myModal");
var span = document.getElementsByClassName("close")[0];

document.getElementById("submit").addEventListener("click", async (e) => {
  e.preventDefault();
  const button = e.target;

  // const name = document.getElementById("name").value.toString();

  button.disabled = true;
  calculate();

  button.disabled = false;

  return false;
});

function calculate() {
  if (
    age.value == "" ||
    height.value == "" ||
    weight.value == "" ||
    (male.checked == false && female.checked == false)
  ) {
    modal.style.display = "block";
    modalText.innerHTML = `All fields are required!`;
  } else {
    countBmi();
  }
}

async function countBmi() {
  var p = [age.value, height.value, weight.value];
  if (male.checked) {
    p.push("male");
  } else if (female.checked) {
    p.push("female");
  }

  const bmi_data = await todo_backend.calculateBMI(
    parseFloat(p[2]),
    parseFloat(p[1])
  );

  const bmi = bmi_data[0];
  const result = bmi_data[1];

  resultArea.style.display = "block";
  document.querySelector(
    ".comment"
  ).innerHTML = `You are <span id="comment">${result}</span>`;
  document.querySelector("#result").innerHTML = bmi.toFixed(2);
}

// When the user clicks on <span> (x), close the modal
span.onclick = function () {
  modal.style.display = "none";
};

// When the user clicks anywhere outside of the modal, close it
window.onclick = function (event) {
  if (event.target == modal) {
    modal.style.display = "none";
  }
};

```
`main.css`
```css
/* Import Google Fonts - Poppins and Roboto */
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap");

/* Root Variables for Colors and Shadows */
:root {
  --primary-color: #6c5ce7;
  --secondary-color: #fdcb6e;
  --background-color: #f8f9fa;
  --text-color: #2d3436;
  --border-color: #fd79a8;
  --modal-shadow-color: #636e72;
  --error-color: #d63031;
  --success-color: #00b894;
  --link-color: #0984e3;
  --box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  --hover-box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: var(--background-color);
  color: var(--text-color);
}

.box {
  width: 500px;
  background: #fff;
  border-radius: 15px;
  text-align: center;
  box-shadow: var(--box-shadow);
  overflow: hidden;
  transition: box-shadow 0.3s ease;
}

.box:hover {
  box-shadow: var(--hover-box-shadow);
}

h1 {
  color: var(--primary-color);
  font-weight: 700;
  font-size: 36px;
  padding: 30px 0;
  font-family: "Poppins", sans-serif;
  background-color: var(--secondary-color);
  margin-bottom: 20px;
}

.content {
  padding: 0 30px;
}

.input,
.inputW,
.inputH {
  background: #fff;
  box-shadow: var(--box-shadow);
  border-radius: 12px;
  padding: 20px 0;
  margin-bottom: 20px;
  transition: transform 0.2s ease;
}

.input:hover,
.inputW:hover,
.inputH:hover {
  transform: translateY(-5px);
}

.input label,
.inputW label,
.inputH label {
  display: block;
  font-size: 18px;
  font-weight: 600;
  color: var(--text-color);
  margin-bottom: 10px;
}

.input input,
.inputW input,
.inputH input {
  outline: none;
  border: 2px solid var(--border-color);
  border-radius: 8px;
  width: 80%;
  text-align: center;
  font-size: 24px;
  padding: 10px;
  margin-bottom: 10px;
  transition: border-color 0.3s ease;
}

.input input:focus,
.inputW input:focus,
.inputH input:focus {
  border-color: var(--primary-color);
}

button.calculate {
  cursor: pointer;
  font-family: "Nunito", sans-serif;
  color: #fff;
  background-image: linear-gradient(
    45deg,
    var(--primary-color),
    var(--secondary-color)
  );
  font-size: 18px;
  border-radius: 8px;
  padding: 15px 0;
  width: 90%;
  outline: none;
  border: none;
  transition: background-color 0.3s ease, transform 0.2s ease;
  margin-bottom: 20px;
}

button.calculate:hover {
  background-color: var(--link-color);
  transform: scale(1.05);
}

.result {
  padding: 15px 20px;
  background-color: var(--background-color);
  border-radius: 12px;
  box-shadow: var(--box-shadow);
  margin-bottom: 20px;
}

.result p {
  font-weight: 600;
  font-size: 22px;
  margin-bottom: 10px;
}

.result #result {
  font-size: 36px;
  font-weight: 900;
  color: var(--primary-color);
  background-color: #eaeaea;
  display: inline-block;
  padding: 10px 25px;
  border-radius: 55px;
  box-shadow: var(--box-shadow);
}

#comment {
  color: var(--primary-color);
  font-weight: 800;
}

.comment {
  display: none;
  border: dashed 1px var(--border-color);
  border-radius: 7px;
  padding: 5px;
}

.footer {
  display: flex;
  justify-content: start;
  align-items: center;
  padding: 10px 15px;
}

.footer-text {
  text-decoration: none;
  color: rgba(40, 40, 40, 0.8);
  font-weight: 200;
  font-size: 14px;
  transition: color 0.3s ease;
}

.footer-text:hover {
  color: var(--link-color);
}

.gender {
  display: flex;
  justify-content: space-around;
  align-items: center;
  align-content: center;
  background: #fff;
  box-shadow: var(--box-shadow);
  border-radius: 12px;
  padding: 20px 0;
  margin-bottom: 20px;
}

/* Style for Radio */
.gender .container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-top: 7px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.gender .container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
  border-radius: 50%;
  transition: background-color 0.3s ease;
}

.gender .container:hover input ~ .checkmark {
  background-color: #ccc;
}

.gender .container input:checked ~ .checkmark {
  background-color: var(--primary-color);
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.gender .container input:checked ~ .checkmark:after {
  display: block;
}

.gender .container .checkmark:after {
  top: 9px;
  left: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: white;
}

.containerHW {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

/* Modal Styles */
.modal {
  display: none;
  position: fixed;
  z-index: 1;
  padding-top: 100px;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
  padding-top: 300px;
  transition: opacity 0.4s ease, visibility 0.4s ease;
}

.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 600px;
  border-radius: 10px;
  box-shadow: var(--box-shadow);
  transition: transform 0.3s ease-out;
}

#modalText {
  padding-top: 8px;
  padding-right: 5px;
  font-size: 18px;
  font-family: "Poppins", sans-serif;
  color: var(--text-color);
}

.modal-wrong {
  border: 2px solid var(--error-color);
}

.modal-correct {
  border: 2px solid var(--success-color);
}

.close {
  color: #aaaaaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
  transition: color 0.3s ease;
}

.close:hover {
  color: var(--error-color);
}

.linkDownload {
  position: fixed;
  background-color: var(--secondary-color);
  margin: 20px;
  padding: 10px 10px;
  left: 0;
  border-radius: 5px;
  bottom: 0;
  font-size: 16px;
  font-weight: 400;
  color: #fff;
  text-decoration: none;
  text-transform: capitalize;
  transition: background-color 0.43s ease-in-out;
}

.linkDownload:hover {
  background-color: var(--primary-color);
}

@media (max-width: 700px) {
  .box {
    width: 380px;
  }

  .input label,
  .inputH label,
  .inputW label {
    font-size: 18px;
  }

  .input input,
  .inputH input,
  .inputW input {
    font-size: 24px;
  }

  .modal-content {
    width: 380px;
  }
}

```
