<template>
    <div class="container">
        <div class="topbar">
            <input type="checkbox"
                    data-ref="selectAll"
                    role="checkbox"
                    aria-checked="false"
                    aria-labelledby="selectAllLabel"
                    v-model="selectAll">
            <label for="selectAll"
                    class="topbar__label"
                    id="selectAllLabel"
                    v-if="selectedFiles.length">Selected {{ selectedFiles.length || '' }}</label>
            <label for="selectAll"
                    class="topbar__label"
                    id="selectAllLabel"
                    v-else>None Selected</label>

            <button class="topbar__download"
                    v-if="selectedFiles.length"
                    @click="downloadFiles">Download Selected</button>
        </div>
        <table class="table">
            <tr class="table__row">
                <th class="table__heading">&nbsp;</th>
                <th class="table__heading">Name</th>
                <th class="table__heading">Device</th>
                <th class="table__heading">Path</th>
                <th class="table__heading">Status</th>
            </tr>
            <app-file v-for="(file, index) in files"
                      :key="index"
                      :file="file"
                      :selectAll="selectAll"
                      :selectedFiles="selectedFiles"
                      @handleCheckbox="handleCheckbox"></app-file>
        </table>
    </div>
</template>

<script>
import File from './File.vue';

export default {
  name: 'FileList',
  components: {
    'app-file': File,
  },
  data() {
    return {
      selectAll: false,
      files: [
        {
          name: 'smss.exe', device: 'Stark', path: '\\Device\\HarddiskVolume2\\Windows\\System32\\smss.exe', status: 'scheduled',
        },
        {
          name: 'netsh.exe', device: 'Targaryen', path: '\\Device\\HarddiskVolume2\\Windows\\System32\\netsh.exe', status: 'available',
        },
        {
          name: 'uxtheme.dll', device: 'Lanniester', path: '\\Device\\HarddiskVolume1\\Windows\\System32\\uxtheme.dll', status: 'available',
        },
        {
          name: 'cryptbase.dll', device: 'Martell', path: '\\Device\\HarddiskVolume1\\Windows\\System32\\cryptbase.dll', status: 'scheduled',
        },
        {
          name: '7za.exe', device: 'Baratheon', path: '\\Device\\HarddiskVolume1\\temp\\7za.exe', status: 'scheduled',
        },
      ],
      selectedFiles: [],
    };
  },
  computed: {
    availableFiles() {
      return this.files.filter((file) => file.status === 'available');
    },
  },
  methods: {
    handleCheckbox(file) {
      if (this.selectedFiles.indexOf(file) > -1) {
        this.selectedFiles.splice(this.selectedFiles.indexOf(file), 1);
      } else {
        this.selectedFiles.push(file);
      }
    },

    downloadFiles() {
      let str = 'Downloading the following file(s):\n';

      this.selectedFiles.forEach((file) => {
        str += `Device: ${file.device}\nPath: ${file.path}\n\n`;
      });

      alert(str);
    },

    setAria(el, attr, val) {
      el.setAttribute(attr, val);
    },

    handleSelectAll(indeterminateState, state) {
      const selectAllCheckbox = document.querySelector('[data-ref="selectAll"]');

      selectAllCheckbox.indeterminate = indeterminateState;
      selectAllCheckbox.setAttribute('aria-checked', state);
    },
  },
  watch: {
    selectedFiles() {
      if (this.selectedFiles.length
          && this.selectedFiles.length < this.availableFiles.length) {
        this.handleSelectAll(true, 'mixed');
      } else if (this.selectedFiles.length
          && this.selectedFiles.length === this.availableFiles.length) {
        this.handleSelectAll(false, 'true');
        this.selectAll = true;
      } else {
        this.handleSelectAll(false, 'false');
        this.selectAll = false;
      }
    },

    selectAll() {
      if (this.selectAll) {
        this.selectedFiles = this.availableFiles.filter(
          (file) => file.status === 'available',
        );
      } else {
        this.selectedFiles = [];
      }
    },
  },
};
</script>

<style lang="scss">
    .container {
        margin: 0 auto;
        width: 75vw;
    }

    .topbar {
        padding: 1rem 2rem;

        &__label {
            font-size: 1rem;
            margin: 0 2rem 0 1rem;
        }

        &__download {
            background: none;
            border: none;
            color: #2c3e50;
            cursor: pointer;
            font-family: Avenir, Helvetica, Arial, sans-serif;
            font-size: 1rem;
        }
    }

    .table {
        border: 1px solid lightgray;
        border-collapse: collapse;

        overflow-x: scroll;
        text-align: left;
        width: 100%;

        &__heading {
            border-bottom: 1px solid lightgray;
            font-size: 1rem;
            padding: 1rem 2rem;
        }
    }
</style>
