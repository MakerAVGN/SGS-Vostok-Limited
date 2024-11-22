<template>
  <div id="app">
    <form>
      <label for="city">Город</label>
      <select v-model="selectedCity" @change="updateFactories">
        <option value="">Выберите город</option>
        <option v-for="city in Object.keys(data)" :key="city" :value="city">
          {{ city }}
        </option>
      </select>

      <label for="factory">Цех</label>
      <select v-model="selectedFactory" @change="updateEmployees">
        <option value="">Выберите цех</option>
        <option v-for="factory in factories" :key="factory" :value="factory">
          {{ factory }}
        </option>
      </select>

      <label for="employee">Сотрудник</label>
      <select v-model="selectedEmployee">
        <option value="">Выберите сотрудника</option>
        <option v-for="employee in employees" :key="employee" :value="employee">
          {{ employee }}
        </option>
      </select>

      <label for="team">Бригада</label>
      <select v-model="selectedTeam">
        <option value="1">Бригада 1</option>
        <option value="2">Бригада 2</option>
      </select>

      <label for="shift">Смена</label>
      <select v-model="selectedShift">
        <option value="day">Дневная смена</option>
        <option value="night">Ночная смена</option>
      </select>

      <button type="button" @click="saveToCookie">Сохранить</button>
      <button type="button" @click="loadFromCookie">Загрузить из Cookie</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: {
        Москва: {
          Цех1: ["Иванов", "Петров"],
          Цех2: ["Сидоров", "Кузнецов"],
        },
        СанктПетербург: {
          Цех3: ["Попов", "Смирнов"],
          Цех4: ["Морозов", "Васильев"],
        },
      },
      selectedCity: "",
      selectedFactory: "",
      selectedEmployee: "",
      selectedTeam: "1",
      selectedShift: "day",
      factories: [],
      employees: [],
    };
  },
  methods: {
    updateFactories() {
      if (this.selectedCity && this.data[this.selectedCity]) {
        this.factories = Object.keys(this.data[this.selectedCity]);
      } else {
        this.factories = [];
      }
      this.selectedFactory = "";
      this.selectedEmployee = "";
      this.employees = [];
    },
    updateEmployees() {
      if (
        this.selectedCity &&
        this.selectedFactory &&
        this.data[this.selectedCity][this.selectedFactory]
      ) {
        this.employees = this.data[this.selectedCity][this.selectedFactory];
      } else {
        this.employees = [];
      }
      this.selectedEmployee = "";
    },
    saveToCookie() {
      const formData = {
        city: this.selectedCity,
        factory: this.selectedFactory,
        employee: this.selectedEmployee,
        team: this.selectedTeam,
        shift: this.selectedShift,
      };
      document.cookie = `formData=${JSON.stringify(
        formData
      )};path=/;max-age=3600`;
      alert("Данные сохранены в Cookie!");
    },
    getCookie(name) {
      const value = `; ${document.cookie}`;
      const parts = value.split(`; ${name}=`);
      if (parts.length === 2) return parts.pop().split(";").shift();
    },
    loadFromCookie() {
      const savedData = this.getCookie("formData");
      if (savedData) {
        const parsedData = JSON.parse(savedData);
        this.selectedCity = parsedData.city;
        this.updateFactories();

        setTimeout(() => {
          this.selectedFactory = parsedData.factory;
          this.updateEmployees();

          setTimeout(() => {
            this.selectedEmployee = parsedData.employee;
            this.selectedTeam = parsedData.team;
            this.selectedShift = parsedData.shift;
          }, 300);
        }, 300);
      } else {
        alert("Данные в Cookie отсутствуют!");
      }
    },
  },
};
</script>

<style>
form {
  max-width: 500px;
  margin: 0 auto;
  font-family: Arial, sans-serif;
}

label,
select {
  display: block;
  margin-bottom: 10px;
}
button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  cursor: pointer;
  margin: 20px;
}
button:hover {
  background-color: #0056b3;
}
</style>
