<template>
  <div id="app-to-do">
    <header>
      <h1>Classic ToDo in Vue</h1>
    </header>
    <section>
      <h2>The best tasks are finished tasks.</h2>
      <div class="input-wrapper">
        <SyncfusionTextBox
          css-class="curr-input-normal-outlined"
          floatLabelType="Auto"
          label="...but first write them here!"
          v-model="enteredTaskValue"
          @keydown.native.enter="addTask"
          :show-clear-button="showClearButton"
        ></SyncfusionTextBox>
      </div>

      <SyncfusionButton
        css-class="curr-btn curr-btn-normal-filled-primary curr-icon-after-activity"
        @click.native="addTask"
        >Add Task</SyncfusionButton
      >

      <div class="manage-list">
        <SyncfusionButton
          css-class="curr-btn curr-btn-normal-outlined-primary curr-icon-after-activity"
          @click.native="toggleList"
          v-if="tasks.length > 0"
          >{{ listStatusCaption }}</SyncfusionButton
        >

        <SyncfusionButton
          css-class="curr-btn curr-btn-normal-outlined-primary curr-icon-after-activity"
          @click.native="clearList"
          v-if="tasks.length > 0"
          >Clear List</SyncfusionButton
        >
      </div>

      <ejs-grid
        v-if="tasks.length > 0"
        ref="grid"
        :dataSource="tasks"
        :editSettings="editSettings"
        :toolbar="toolbar"
        :allowRowDragAndDrop="true"
      >
        <e-column field="task" text-align="Center" width="90" />
      </ejs-grid>
    </section>
  </div>
</template>

<script>
import Vue from 'vue';
import SyncfusionTextBox from 'currenda-generic-vue/src/components/generic/SyncfusionTextBox.vue';
import SyncfusionButton from 'currenda-generic-vue/src/components/generic/SyncfusionButton.vue';
import {
  GridPlugin,
  RowDD,
  Page,
  Toolbar,
  Edit,
} from '@syncfusion/ej2-vue-grids';

Vue.use(GridPlugin);

export default {
  name: 'AppToDo',
  components: {
    SyncfusionTextBox,
    SyncfusionButton,
  },
  data() {
    return {
      enteredTaskValue: '',
      tasks: [],
      listStatusCaption: 'Hide List',
      showClearButton: true,
      editSettings: {
        allowEditing: true,
        allowDeleting: true,
        mode: 'Normal',
      },
      toolbar: ['Edit', 'Delete', 'Update', 'Cancel'],
    };
  },

  provide: {
    grid: [Page, RowDD, Edit, Toolbar],
  },

  methods: {
    addTask() {
      if (this.enteredTaskValue !== '') {
        this.tasks.push({ task: this.enteredTaskValue });
        this.enteredTaskValue = '';
        if (this.tasks.length > 1) {
          let gridIns = this.$refs.grid.ej2Instances;
          gridIns.refresh();
        }
      }
    },

    toggleDone(event) {
      event.target.classList.toggle('line');
      console.log(event);
    },

    toggleList() {
      this.listStatusCaption =
        this.listStatusCaption === 'Hide List' ? 'Show List' : 'Hide List';
    },

    clearList() {
      this.tasks = [];
    },
  },
};
</script>

<style>
@import 'currenda-generic-vue/src/styles/bapi.scss';
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap');

@font-face {
  font-family: 'cdsicon';
  src: url(./assets/fonts/cdsicon.woff) format('woff'),
    url(./assets/fonts/cdsicon.woff2) format('woff2');
  font-style: normal;
  font-weight: 400;
}

.e-grid .e-gridheader {
  display: none;
}
</style>

<style lang="scss" scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app-to-do {
  --font: Roboto, sans-serif;
  --textColor: #374961;
  --mainColor: #d44108;
  --secondColor: #eee;
  --hoverColor: #9c3900;
  font-family: var(--font);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--textColor);

  header {
    width: 40%;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
    margin: 3rem auto;
    border-radius: 10px;
    padding: 1rem;
    background-color: var(--mainColor);
    color: white;
    text-align: center;
  }

  section {
    width: 40%;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
    margin: 3rem auto;
    border-radius: 10px;
    padding: 1rem;
    text-align: center;

    .input-wrapper {
      width: 60%;
      margin: 20px auto;
    }

    h2 {
      font-size: 1.3rem;
      // border-bottom: 3px solid #ccc;
      color: var(--mainColor);
      margin: 0 0 1rem 0;
    }

    ul {
      display: flex;
      flex-direction: column;
      align-items: center;
      list-style: none;
      margin: 0.5rem 0;
      padding: 0;
    }

    li {
      margin: 5px;
      font-size: 1.25rem;
      // font-weight: bold;
      background-color: var(--mainColor);
      padding: 10px 10px 10px 50px;
      color: white;
      border-radius: 7px;

      box-icon {
        margin: 0;
        vertical-align: bottom;
        cursor: pointer;
      }

      .icon-done {
        margin-left: 20px;
      }
    }

    .line {
      text-decoration: line-through;
      color: #ffa673;
    }

    // input {
    //   width: 30%;
    //   padding: 5px;
    //   font: inherit;
    //   border: 1px solid #ccc;
    //   border-radius: 5px;
    // }

    // input:focus {
    //   outline: none;
    //   border-color: var(--secondColor);
    //   background-color: var(--secondColor);
    // }

    // button {
    //   display: block;
    //   margin: 10px auto;
    //   font: inherit;
    //   cursor: pointer;
    //   border: 1px solid var(--mainColor);
    //   border-radius: 5px;
    //   background-color: var(--mainColor);
    //   color: white;
    //   padding: 5px 15px 7px;
    //   box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);

    //   box-icon {
    //     vertical-align: bottom;
    //   }
    // }

    // button:hover,
    // button:active {
    //   background-color: var(--hoverColor);
    //   border-color: var(--hoverColor);
    //   box-shadow: 1px 1px 4px rgba(0, 0, 0, 0.26);
    // }

    .manage-list {
      display: flex;
      justify-content: center;

      button {
        margin: 15px 10px 10px;
      }
    }
  }
}
</style>
