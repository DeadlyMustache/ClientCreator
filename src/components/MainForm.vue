<template>
  <div class="container">
    <form @submit.prevent="onSubmit"> 
      <div class="formGroup">              
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                :class="{invalid: $v.surname.$dirty && (!$v.surname.required || !$v.surname.alpha)}"
                placeholder="Фамилия" 
                id="inputSurname" 
                v-model.trim="surname" 
                autocomplete="off"
            />
            <label for="inputSurname" class="form__label">Фамилия</label>
            <small class="error__text" v-if="$v.surname.$dirty && !$v.surname.required">
                Введите фамилию!
            </small>
            <small class="error__text" v-else-if="$v.surname.$dirty && !$v.surname.alpha">
                Числа не допустимы!
            </small>
        </div>
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                :class="{invalid: $v.name.$dirty && (!$v.name.required || !$v.name.alpha)}"
                placeholder="Имя" 
                id="inputName" 
                v-model.trim="name" 
                autocomplete="off"
            />
            <label for="inputName" class="form__label">Имя</label>
            <small class="error__text" v-if="$v.name.$dirty && !$v.name.required">
                Введите имя!
            </small>
            <small class="error__text" v-else-if="$v.name.$dirty && !$v.name.alpha">
                Числа не допустимы!
            </small>
        </div>
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                :class="{invalid: $v.father_name.$dirty && !$v.father_name.alpha}"
                placeholder="Отчество" 
                id="inputFatherName" 
                v-model.trim="father_name" 
                autocomplete="off"
            />
            <label for="inputFatherName" class="form__label">Отчество</label>
            <small class="error__text" v-if="$v.father_name.$dirty && !$v.father_name.alpha">
                Числа не допустимы!
            </small>
        </div>
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                :class="{invalid: $v.phone_number.$dirty && (!$v.phone_number.numeric || !$v.phone_number.minLength || !$v.phone_number.maxLength || !$v.phone_number.required)}"
                placeholder="Номер телефона" 
                id="inputPhoneNumber" 
                v-model.trim="phone_number" 
                autocomplete="off"
            />
            <label for="inputPhoneNumber" class="form__label">Номер телефона</label>
            <small class="error__text" v-if="$v.phone_number.$dirty && !$v.phone_number.required">
                Введите телефон!
            </small>
            <small class="error__text" v-else-if="$v.phone_number.$dirty && !$v.phone_number.numeric">
                Буквы не допустимы!
            </small>
            <small class="error__text" v-else-if="$v.phone_number.$dirty && (!$v.phone_number.minLength || !$v.phone_number.maxLength)">
                Введите {{$v.phone_number.$params.minLength.min}} цифр! Сейчас вы ввели {{phone_number.length}}.
            </small>
            <small class="error__text" v-else-if="$v.phone_number.$dirty && !$v.phone_number.mustStartWith7">
                Номер должен начинаться с цифры 7!
            </small>
        </div>
        <div class="form__group field">    
            <input class="form__field" type="date" id="inputBirthDate" v-model="birth_date">
            <label class="form__label" for="inputBirthDate">Дата рождения</label>
            <small class="error__text" v-if="$v.birth_date.$dirty && !$v.birth_date.required">
                Выберите дату рождения!
            </small>
            
        </div>
        <div  class="form__group field">
            <fieldset>
            <legend>Пол</legend>
            <div class="toggle">
                <input 
                    type="radio" 
                    name="radioGender" 
                    id="radioGenderMale" 
                    value="genderMale" 
                    v-model="gender"
                >
                <label for="radioGenderMale">Мужчина</label>
                <input 
                    type="radio" 
                    name="radioGender" 
                    id="radioGenderFemale" 
                    value="genderFemale" 
                    v-model="gender"
                >
                <label for="radioGenderFemale">Женщина</label>
                <input 
                    type="radio" 
                    name="radioGender" 
                    id="radioGenderOther" 
                    value="genderOther" 
                    v-model="gender"
                >
                <label for="radioGenderOther">Другое</label>
            </div>
            </fieldset>            
        </div>

        <!--ГРУППЫ КЛИЕНТА-->
        <div class="form__group field">
            <label for="selectClientGroups">Группы клиента</label>
            <select class="multiselect" id="selectClientGroups" v-model="client_groups" multiple>
                <option value="groupVIP">VIP</option>
                <option value="groupProblematic">Проблемные</option>
                <option value="groupOMS">ОМС</option>
            </select>
            <small class="error__text" v-if="$v.client_groups.$dirty && !$v.client_groups.required">
                Выберите группу(ы) клиента!
            </small>
        </div>

        <!--НАЗНАЧЕННЫЙ ВРАЧ-->
        <div class="form__group field">
            <label for="selectAssignedDoctor">Назначенный врач</label>
            <select class="select" id="selectAssignedDoctor" v-model="assigned_doctor">
                <option value="mdIvanov">Иванов</option>
                <option value="mdZaharov">Захаров</option>
                <option value="mdZaharova">Чернышова</option>
            </select>
        </div>

        <div class="form__group field checkbox__group">
          <input class="checkbox" type="checkbox" id="checkboxDoNotSendSMS" v-model="dont_send_SMS">
          <label class="checkbox__label" for="checkboxDoNotSendSMS">Не отправлять СМС</label>
        </div>
      </div> 

      <!-- ADDRESS -->
      <div class="formGroup">
        <!--ИНДЕКС-->
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                :class="{invalid: $v.index.$dirty && !$v.index.numeric}"
                placeholder="Индекс" 
                id="inputIndex" 
                v-model.trim="index" 
                autocomplete="off"
            />
            <label for="inputIndex" class="form__label">Индекс</label>
            <small class="error__text" v-if="$v.index.$dirty && !$v.index.numeric">
                Буквы не допустимы!
            </small>
        </div>

        <!--СТРАНА-->
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                :class="{invalid: $v.country.$dirty && !$v.country.alpha}"
                placeholder="Страна" 
                id="inputCountry" 
                v-model.trim="country" 
                autocomplete="off"
            />
            <label for="inputCountry" class="form__label">Страна</label>
           <small class="error__text" v-if="$v.country.$dirty && !$v.country.alpha">
                Числа не допустимы!
            </small>
        </div>

        <!--ГОРОД-->
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                :class="{invalid: $v.city.$dirty && (!$v.city.required || !$v.city.alpha)}"
                placeholder="Город" 
                id="inputCity" 
                v-model.trim="city" 
                autocomplete="off"
            />
            <label for="inputCity" class="form__label">Город</label>
            <small class="error__text" v-if="$v.city.$dirty && !$v.city.required">
                Введите название города(села/деревни/аула/кишлака/хутора/поселка городског типа/конгломерации/...)!
            </small>
            <small class="error__text" v-else-if="$v.city.$dirty && !$v.city.alpha">
                Числа не допустимы!
            </small>
        </div>

        <!--УЛИЦА-->
        <div class="form__group field">
          <input 
            type="input" 
            class="form__field" 
            :class="{invalid: $v.street.$dirty && !$v.street.alpha}"
            placeholder="Улица" 
            id="inputStreet" 
            v-model.trim="street" 
            autocomplete="off"
          />
          <label for="inputStreet" class="form__label">Улица</label>
          <small class="error__text" v-if="$v.street.$dirty && !$v.street.alpha">
            Числа не допустимы!
          </small>
        </div>

        <!--ДОМ-->
        <div class="form__group field">
          <input 
            type="input" 
            class="form__field"            
            placeholder="Дом" 
            id="inputHome" 
            v-model.trim="home" 
            autocomplete="off"
          />
          <label for="inputHome" class="form__label">Дом</label>
        </div>
      </div>

      <!--ПАСПОРТ-->
      <div class="formGroup">
        <!--ТИП ДОКУМЕНТА-->
        <div class="form__group field">
            <label for="selectDocumentType">Тип документа</label>
            <select class="select" id="selectDocumentType" v-model="document_type">
                <option value="docPassport">Паспорт</option>
                <option value="docBirthRegistration">Свидетельство о рождении</option>
                <option value="mdZdocDriverLicenseaharova">Вод. удостоверение</option>
            </select>
            <small class="error__text" v-if="$v.document_type.$dirty && !$v.document_type.required">
                Выберите тип документа!
            </small>
        </div>

        <!--СЕРИЯ-->
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                :class="{invalid: $v.series.$dirty && !$v.series.numeric}"
                placeholder="Серия" 
                id="inputSeries" 
                v-model.trim="series" 
                autocomplete="off"
            />
            <label for="inputSeries" class="form__label">Серия</label>
            <small class="error__text" v-if="$v.series.$dirty && !$v.series.numeric">
                Буквы не допустимы!
            </small>
        </div>

        <!--НОМЕР-->
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                :class="{invalid: $v.number.$dirty && !$v.number.numeric}"
                placeholder="Номер" 
                id="inputNumber" 
                v-model.trim="number" 
                autocomplete="off"
            />
            <label for="inputNumber" class="form__label">Номер</label>
            <small class="error__text" v-if="$v.number.$dirty && !$v.number.numeric">
                Буквы не допустимы!
            </small>
        </div>

        <!--КЕМ ВЫДАН-->
        <div class="form__group field">
            <input 
                type="input" 
                class="form__field" 
                placeholder="Кем выдан" 
                id="inputWhoGave" 
                v-model.trim="who_gave" 
                autocomplete="off"
            />
            <label for="inputWhoGave" class="form__label">Кем выдан</label>
        </div>

        <div class="form__group field">    
            <input class="form__field" type="date" id="inputGettingDate" v-model="getting_date">
            <label class="form__label" for="inputGettingDate">Дата выдачи</label>
            <small class="error__text" v-if="$v.getting_date.$dirty && !$v.getting_date.required">
                Выберите дату выдачи!
            </small>
        </div>
      </div>
      <div class="formGroup">
        <button class="submit__button" type="submit">СОЗДАТЬ</button>
      </div>
      
    </form>
  </div>
</template>

<script>
import { minLength, required, alpha, numeric, maxLength } from 'vuelidate/lib/validators'
const mustStartWith7 = (value) => value.charAt(0) == '7'
export default {
  name: 'MainForm',
  data() {
    return {
        surname: "",
        name: "",
        father_name: "",
        phone_number: "",
        birth_date: "",
        gender: "",
        client_groups: [],
        assigned_doctor: "",
        dont_send_SMS: false,
        //address
        index: "",
        country: "",
        region: "",
        city: "",
        street: "",
        home: "",
        //passport
        document_type: "",
        series: "",
        number: "",
        who_gave: "",
        getting_date: ""      
    }
  },
  validations: {
      surname: { required, alpha },
      name: { required, alpha },
      father_name: { alpha },
      birth_date: { required },
      phone_number: { required, numeric, minLength: minLength(11), maxLength: maxLength(11), mustStartWith7 },
      gender: {  },
      client_groups: { required },
      assigned_doctor: {  },
      index: { numeric },
      country: { alpha },
      city: { required, alpha },
      street: { alpha },
      home: { },
      document_type: { required },
      series: { numeric },
      number: { numeric },
      getting_date: { required }
  },
  components: {
  },
  methods: {
    onSubmit(event) {
      if (this.$v.$invalid) {
        this.$v.$touch();
        return;
      }
      alert("Клиент создан!");
      document.location.reload();
    }    
  }
}
</script>


<style scoped lang="scss">
.container {
  width: 100%;
  max-width: 930px;
  padding: 0 15px;
  margin: 0 auto;
}

form {
  width: 100%;
  font-family: 'Poppins', sans-serif;
  font-size: 1.5rem;
  background-color:#fff;
  border-radius: 10px;
  padding: 1rem 0;
  margin: 1rem auto;
}

input, select, label {
  color: #333;
}

.submit__button {
  width: 100%;
  padding: .5rem;
  font-size: 1.33rem;
  font-weight: 400;
  text-transform: uppercase;
  color: #333;

  border-radius: 5px;
  border: 2px solid #333;
}

.formGroup {
  width: 100%;
  max-width: 630px;
  margin: 1rem auto;
  padding: 1rem;

  background-color: darken(#fff, 15);
  border-radius: 10px;
     
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;    
}

.error__text {
  color: red;
  font-style: italic;
  padding: 5px 0;
}

/* INPUTS */

$primary: #0075FF;
$black: #333;

.form__group {
  position: relative;
  padding: 15px 0 0;
  margin-top: 10px;
  width: 100%;
  display: flex;
  flex-direction: column;
  &.checkbox__group {
    flex-direction: row;
    align-items: center;
  }
}


.form__field {
  font-family: inherit;
  width: 100%;
  border: 0;
  border-radius: 5px;
  border-bottom: 2px solid $black;
  outline: 0;
  font-size: 1.33rem;
  color: $black;
  padding: 7px 0;
  background: transparent;
  transition: border-color 0.2s;

  &::placeholder {
    color: transparent;
  }

  &:placeholder-shown ~ .form__label {
    font-size: 1.33rem;
    cursor: text;
    top: 1.66rem;
  }
}

.form__label {
  position: absolute;
  top: 0;
  display: block;
  transition: 0.2s;
  font-size: 1rem;
  color: $black;
}

.form__field:focus {
  ~ .form__label {
    position: absolute;
    top: 0;
    display: block;
    transition: 0.2s;
    font-size: 1rem;
    color: $primary;
    font-weight:700;    
  }
  padding-bottom: 6px;  
  font-weight: 700;
  border-width: 3px;
  border-image: linear-gradient(to right, darken($primary, 10), lighten($primary, 10));
  border-image-slice: 1;
}
.form__field{
  &:required,&:invalid { box-shadow:none; }
}

/*RADIO*/
$darkNavy: #213140;
$teal1: #66B3FB;
$teal2: #4B9DEA;
$charcoal: #555555;
$gold: #B6985A;

$activeShadow: 0 0 10px rgba($teal1, .5);

@mixin focusOutline {outline: dotted 1px #CCC; outline-offset: .45rem;}
@mixin hideInput {width: 0; height: 0; position: absolute; left: -9999px;}
@mixin breakpoint($point) {
	@if $point == 1100 {
		@media (max-width: 1100px) { @content ; }
	}
	@else if $point == 800 {
		@media (max-width: 800px) { @content ; }
	}
}

fieldset {
  width: 100%;
	margin: 0; 
  padding: .75rem 0; 
	box-sizing: border-box; 
  display: block;
	border: none; 
	min-width: 0;
	background-color: transparent;
	legend {
    margin: 0 0 .5rem; 
    padding: 0;
		width: 100%; 
    float: left; 
    display: table;
		font-size: 1.33rem; 
    line-height: 140%; 
    font-weight: 400; 
    color: #333;	
		+ * {clear: both;}
	}
}

.toggle {
  margin: 0 0 .5rem; 
  box-sizing: border-box;
	font-size: 0;
	display: flex; 
  flex-flow: row wrap;
	justify-content: flex-start; 
  align-items: stretch;
	input {@include hideInput;}
	input + label {
		margin: 0; 
    padding: .75rem 2rem; 
    box-sizing: border-box;
		position: relative; 
    display: inline-block;
		border: solid 1px #DDD; 
    background-color: #FFF;
		font-size: 1rem; 
    line-height: 140%; 
    font-weight: 600; 
    text-align: center;
		box-shadow: 0 0 0 rgba(255,255,255,0);
		transition: 	border-color .15s ease-out, 
					color .25s ease-out, 
					background-color .15s ease-out,
					box-shadow .15s ease-out;
		
		&:first-of-type {border-radius: 6px 0 0 6px; border-right: none;}
		&:last-of-type {border-radius: 0 6px 6px 0; border-left: none;}
	}
	input:hover + label {border-color: $darkNavy;}
	input:checked + label {
		background-color: $teal2;
		color: #FFF;
		box-shadow: $activeShadow;
		border-color: $teal2;
		z-index: 1;
	}
	input:focus + label {@include focusOutline;}

	@include breakpoint(800) {
		input + label {
			padding: .75rem .25rem;
			flex: 0 0 50%;
			display: flex; 
      justify-content: center; 
      align-items: center;
		}
	}
}

/*=====================CHECKBOX====================*/
.checkbox {
  &:checked {
    & + .checkbox__label {
      color: black;
    }
  }
}
.checkbox__label {
  margin-left: .66rem;
  font-size: 1.33rem;
  color: gray;
  cursor: pointer;  

  transition: color .3s ease;
  &:hover{
    color: #0075FF;
  }  
}

/* ====================== SELECT ====================*/
.select {
  margin-top: .66rem;
  width: 100%;
  font-size: 1.33rem;
  border: 2px solid #333;
  border-radius: 5px;

  &:focus, &:active {
    border: 2px solid #0075FF;
    color: white;
    border-radius: 5px 5px 0 0;
    background: #0075FF;
  }
}

/*=============== MULTISELECT ===============*/
.multiselect{
  margin-top: .66rem;
  width: 100%;
  font-size: 1.33rem;
  border: 2px solid gray;
  border-radius: 5px;

  &::-webkit-scrollbar {
    display: none;
  }
}

/*================= CHECKBOX =============*/
.checkbox {
  &:checked {
    & + .checkbox__label {
      color: black;
    }
  }
}
.checkbox__label {
  margin-left: .66rem;
  font-size: 1.33rem;
  color: gray;
  cursor: pointer;  

  transition: color .3s ease;
  &:hover{
    color: #0075FF;
  }
  
}
</style>
