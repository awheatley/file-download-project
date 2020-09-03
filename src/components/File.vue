<template>
    <tr :class="['table__row', {selected: selected}]">
        <td class="table__cell -checkbox">
            <input type="checkbox"
                    class="table__checkbox"
                    :id="file.name"
                    role="checkbox"
                    aria-checked="false"
                    :aria-labelledby="file.name.split('.')[0]"
                    :disabled="file.status !== 'available'"
                    v-model="selected"
                    @click="handleCheckbox"/>
        </td>
        <td class="table__cell" :id="file.name.split('.')[0]">{{ file.name }}</td>
        <td class="table__cell">{{ file.device }}</td>
        <td class="table__cell">{{ file.path }}</td>
        <td :class="['table__cell', { available: file.status === 'available' }]">{{ file.status|capitalize }}
        </td>
    </tr>
</template>

<script>
export default {
  props: {
    file: {
      type: Object,
      required: true,
    },
    selectAll: {
      type: Boolean,
      required: true,
    },
    selectedFiles: {
      type: Array,
      required: true,
    },
  },
  filters: {
    capitalize(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
  data() {
    return {
      selected: false,
    };
  },
  methods: {
    handleCheckbox() {
      const fileCheckbox = document.getElementById(`${this.file.name}`);
      const ariaChecked = fileCheckbox.getAttribute('aria-checked');

      fileCheckbox.setAttribute('aria-checked', ariaChecked == 'true' ? 'false' : 'true');
      this.$emit('handleCheckbox', this.file);
    },
  },
  watch: {
    selectAll() {
      if (this.selectAll && this.file.status === 'available') {
        this.selected = this.selectAll;
      }
    },

    selectedFiles() {
      if (this.selectedFiles.length === 0) {
        this.selected = false;
      }
    },
  },
};
</script>

<style lang="scss">
    .table {
        &__row {
            &.selected {
                background-color: #eee;
            }

            &:hover {
                background-color: #f5f5f5;
                cursor: pointer;
            }
        }

        &__cell {
            border-bottom: 1px solid lightgray;
            font-size: 1rem;
            padding: 1rem 2rem;

            &.-checkbox {
                padding-right: 0;
            }

            &.available {
                position: relative;

                &::before {
                    background: #94cc4f;
                    border-radius: 50%;
                    content: '';
                    height: 1rem;
                    left: 0;
                    position: absolute;
                    top: 50%;
                    transform: translateY(-50%);
                    width: 1rem;
                }
            }
        }
    }
</style>
