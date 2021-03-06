<template>
  <div id="app-to-do-syncfusion">
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
        css-class="curr-btn curr-btn-normal-filled-primary curr-icon-after-add_to_list"
        @click.native="addTask"
        >Add Task</SyncfusionButton
      >

      <div class="manage-list">
        <SyncfusionButton
          css-class="curr-btn curr-btn-normal-outlined-primary curr-icon-after-select"
          @click.native="toggleList"
          v-if="tasks.length > 0"
          >{{ listStatusCaption }}</SyncfusionButton
        >

        <SyncfusionButton
          css-class="curr-btn curr-btn-normal-outlined-primary curr-icon-after-close_square"
          @click.native="clearList"
          v-if="tasks.length > 0"
          >Clear List</SyncfusionButton
        >
      </div>

      <ejs-grid
        v-if="tasks.length > 0 && listStatusCaption === 'Hide List'"
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
  name: 'AppToDoSyncfusion',
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

@font-face {
  font-family: 'cdsicon';
  src: url(./assets/fonts/cdsicon.woff) format('woff'),
    url(./assets/fonts/cdsicon.woff2) format('woff2');
  font-style: normal;
  font-weight: 400;
}

body {
  height: auto;
}

.e-grid {
  border-color: transparent;
}

.e-grid .e-gridheader {
  display: none;
}

.e-grid .e-toolbar,
.e-grid .e-toolbar-items {
  background-color: transparent;
}

.e-toolbar .e-toolbar-items .e-toolbar-item .e-tbar-btn.e-btn {
  padding: 3px 6px;
  border-radius: 5px;
}

.e-toolbar .e-tbar-btn.e-btn .e-icons,
.e-toolbar .e-toolbar-items .e-toolbar-item .e-tbar-btn-text {
  color: #d44108;
}

.e-toolbar .e-tbar-btn.e-btn:hover:not([disabled]) .e-icons,
.e-toolbar
  .e-toolbar-items
  .e-toolbar-item:hover:not([disabled])
  .e-tbar-btn-text {
  color: #fff;
}

.e-btn:hover:not([disabled]) {
  background-color: #d44108;
  border-radius: 5px;
  color: #fff;
}
</style>

<style lang="scss" scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app-to-do-syncfusion {
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
    color: var(--secondColor);
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
      color: var(--mainColor);
      margin: 0 0 1rem 0;
    }

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
