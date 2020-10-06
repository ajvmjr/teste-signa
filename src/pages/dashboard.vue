<template>
  <div>
    <header class="cabecalho flex">
      <img src="@/assets/Vector.png" alt="" />
      <input type="text" placeholder="Pesquise por nome ou skills" />
    </header>

    <main class="container flex">
      <div class="container__title-btn flex">
        <h1>Lista de Alunos</h1>
        <button class="container__title-btn__btns" @click="showModal = true">
          Novo
        </button>
      </div>
      <table class="container__table">
        <thead>
          <tr>
            <th><input type="checkbox" name="" /></th>
            <th>Nome</th>
            <th>Email</th>
            <th>Skills</th>
            <th>Adicionado em:</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="student in studentsList" :key="student.id">
            <td><input type="checkbox" name="" :value="student.id" /></td>
            <td>
              <b> {{ student.name }} </b>
            </td>
            <td>{{ student.email }}</td>
            <td>
              <span
                v-for="(skill, i) in student.skills"
                :key="i"
                class="container__table__skills"
                >{{ skill }}</span
              >
            </td>
            <td>{{ student.date }}</td>
          </tr>
        </tbody>
      </table>

      <Modal v-if="showModal">
        <div class="container__modal-content">
          <h1>Cadastrar novo aluno</h1>
          <div class="container__modal-content__content-image">
            <img src="@/assets/ellipse.png" alt="" />
          </div>
          <form @submit="createStudents">
            <div class="container__modal-content__name-email">
              <input
                type="text"
                placeholder="Nome"
                v-model="createStudent.name"
              />
              <input
                type="text"
                placeholder="Email"
                v-model="createStudent.email"
              />
            </div>
            <input
              type="text"
              placeholder="Skills separadas por ,"
              v-model="createStudent.skills"
            />
            <input
              type="text"
              placeholder="Digite a url da foto"
              v-model="createStudent.url"
            />
            <button>Novo</button>
          </form>
        </div>
      </Modal>
    </main>
  </div>
</template>

<script>
import axios from "axios";
import Modal from "@/components/Modal";

export default {
  components: {
    Modal,
  },

  data() {
    return {
      studentsList: [],
      showModal: false,
      createStudent: {
        name: "",
        email: "",
        skills: [],
        url: "",
      },
    };
  },

  mounted() {
    this.listStudents();
  },

  methods: {
    async listStudents() {
      try {
        const { data } = await axios.get("http://localhost:3000/alunos");
        console.log(data);
        this.studentsList = data;
      } catch (err) {
        console.log(err);
      }
    },

    async createStudents(e) {
      e.preventDefault();

      await axios.post("http://localhost:3000/alunos", {
        name: this.createStudent.name,
        email: this.createStudent.email,
        skills: this.createStudent.skills,
        avatar: this.createStudent.url,
      });

      this.createStudent.name = "";
      this.createStudent.email = "";
      this.createStudent.skills = "";
      this.createStudent.url = "";

      this.listStudents();
      this.closeModal(e);
    },

    closeModal(e) {
      e.preventDefault();

      this.showModal = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.flex {
  display: flex;
}

.cabecalho {
  align-items: center;
  border-bottom: 1px solid #ebeff2;
  height: 60px;
  width: 100vw;

  img {
    margin-left: 50px;
  }

  input {
    font-family: "Poppins", sans-serif;
    font-style: normal;
    font-weight: normal;
    font-size: 12px;
    line-height: 18px;
    color: #90a0b7;
    letter-spacing: 0.01em;
    border: 0;
    margin-left: 30px;
    outline: 0;
    width: 200px;
  }
}

.container {
  align-items: center;
  flex-direction: column;
  height: 100%;
  margin: 0 auto;
  width: 100%;

  &__title-btn {
    justify-content: space-between;
    padding-top: 30px;
    width: 80%;

    h1 {
      font-style: normal;
      font-weight: bold;
      font-size: 20px;
      line-height: 30px;
      text-align: right;
      letter-spacing: 0.01em;
      color: #109cf1;
    }

    &__btns {
      background: #109cf1;
      color: white;
      outline: 0;
      border: 0;
      box-shadow: 0px 4px 10px rgba(16, 156, 241, 0.24);
      border-radius: 4px;
      height: 42px;
      width: 160px;
    }
  }

  &__table {
    background: #ffffff;
    width: 100%;

    thead {
      th {
        font-style: normal;
        font-weight: 500;
        font-size: 13px;
        line-height: 19px;
        letter-spacing: 0.01em;
        color: #334d6e;
        opacity: 0.5;

        &:nth-child(even) {
          width: 30%;
        }
      }
    }

    tbody {
      tr {
        td {
          text-align: center;
          font-style: normal;
          font-weight: normal;
          font-size: 13px;
          line-height: 19px;
          letter-spacing: 0.01em;
          color: #707683;
          vertical-align: left;
          height: 64px;
        }
      }
    }

    &__skills {
      font-style: normal;
      font-weight: 500;
      font-size: 13px;
      line-height: 19px;
      letter-spacing: 0.01em;
      color: #109cf1;
      text-align: center;
      padding: 10px 15px;
      border-radius: 5px;
      border: 1px solid #109cf1;
      margin-right: 15px;
    }
  }

  &__modal-content {
    height: 500px;
    width: 30%;

    padding: 15px;
    h1 {
      font-style: normal;
      font-weight: normal;
      font-size: 18px;
      line-height: 27px;

      letter-spacing: 0.01em;

      color: #2c3033;
      margin-bottom: 1px solid #c7c5c5;
    }

    &__content-image {
      display: flex;
      justify-content: center;
      padding-top: 20px;
      padding-bottom: 30px;
    }

    form {
      // background: green;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 50%;

      input {
        border: 1px solid rgba(25, 25, 25, 0.32);
        box-sizing: border-box;
        padding: 15px 45px;
      }

      button {
        background: #109cf1;
        color: white;
        outline: 0;
        border: 0;
        box-shadow: 0px 4px 10px rgba(16, 156, 241, 0.24);
        border-radius: 4px;
        height: 42px;
        width: 160px;
        align-self: flex-end;
      }
    }

    &__name-email {
      display: flex;
      justify-content: space-between;
    }
  }
}
</style>
