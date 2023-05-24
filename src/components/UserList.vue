<template>
  <div class="dropdown-menu">
    <input
      v-if="enableSearch"
      type="search"
      class="form-control search-bar rounded"
      :placeholder="searchPlaceholder"
      aria-describedby="search-addon"
      @input="(event: any) => onSearchHandler(event.target.value?.toLocaleLowerCase())"
    />
    <div v-if="computedList.length">
      <ul class="list-group">
        <li
          class="list-group-item d-flex align-items-center"
          :key="item.id"
          v-for="item in computedList"
        >
          <input
            class="form-check-input"
            type="checkbox"
            id="checkboxNoLabel"
            value=""
            :checked="Boolean(item.isSelected)"
            aria-label="..."
            @click="() => onCheckboxClick(item.id)"
          />
          <div class="rounded-circle user-icon" :class="{ 'active-user': item.isSelected }">
            <img src="../assets/svg/user.svg" alt="Avatar" />
          </div>
          <div class="">
            <h6 class="fw-bold user-name">{{ item.name }}</h6>
            <p class="user-mail">{{ item.email }}</p>
          </div>
        </li>
      </ul>

      <button type="button" class="btn submit-button">{{ captions }}</button>
    </div>
    <div v-else class="d-flex justify-content-center">No User Found</div>
  </div>
</template>

<script lang="ts">
/**
 * Imports
 */
import { computed, ref } from 'vue'
import { toRefs, defineComponent } from 'vue'

export default defineComponent({
  /**
   *  Component Props
   */
  props: {
    list: {
      type: Object,
      require: true,
      default: () => {}
    },
    enableSearch: {
      type: Boolean,
      require: false,
      default: () => true
    },
    onSearch: {
      type: Function,
      require: false,
      default: () => Function
    },
    searchPlaceholder: {
      type: String,
      require: false,
      default: () => ''
    },
    captions: {
      type: String,
      require: false,
      default: () => ''
    }
  },
  /**
   *  Emits Methods
   */
  emits: ['onAssign', 'onSearch'],
  setup(props, context) {
    /**
     *  Props
     */
    const { list, enableSearch, searchPlaceholder, captions } = toRefs(props)

    /**
     *  Computed Properties
     */
    const computedList = computed(() =>
      list.value.sort((x: any, y: any) =>
        x['isSelected'] === y['isSelected'] ? 0 : x['isSelected'] ? -1 : 1
      )
    )

    /**
     * Funtions
     */

    /**
     * user checkbox handler
     * @param id
     */
    const onCheckboxClick = (id: number) => {
      context.emit('onAssign', id)
    }

    /**
     * User search handler
     * @param searchValue
     */
    const onSearchHandler = (searchValue: string) => {
      context.emit('onSearch', searchValue)
    }

    return {
      list,
      enableSearch,
      onCheckboxClick,
      computedList,
      onSearchHandler,
      searchPlaceholder,
      captions
    }
  }
})
</script>

<style scoped>
.btn {
  width: 126px;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  align-items: center;
  padding: 9px 16px;
}
.icon-img {
  min-height: 24px;
  min-width: 24px;
  margin-right: 28px;
  cursor: pointer;
}

.dropdown-menu {
  min-width: 389px;
  padding: 15px;
  transform: translate(-262px, 52px) !important;
  background: #f6f6f6;
  box-shadow: 0px 4px 30px rgba(0, 0, 0, 0.25);
  border-radius: 5px;
}
.list-group-item {
  background: #f6f6f6;
}

.list-group-item {
  border: none;
  padding: 0;
  padding-bottom: 17px;
}
.user-icon {
  width: 75px;
  height: 75px;
  margin-left: 14px;
  margin-right: 10px;
  background: #e9ecef;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}
.active-user {
  border: 2px solid #5227cc;
}

.submit-button {
  background: #e9ecef;
  mix-blend-mode: normal;
  border-radius: 6px;
  padding: 10px 16px;
  width: 100%;
  font-weight: 600;
  font-size: 16px;
  line-height: 18px;
  color: #000000;
}
.user-name {
  font-weight: 600;
  font-size: 20px;
  line-height: 24px;
  margin-bottom: 11px;
}
.user-mail {
  font-weight: 400;
  font-size: 16px;
  line-height: 28px;
  color: #68717a;
  margin-bottom: 0px;
}
.form-check-input {
  cursor: pointer;
  background: #ffffff;
  border: 1px solid #abb5be;
  border-radius: 2px;
}
.form-check-input:checked {
  background-color: #7749f8;
  border-color: #7749f8;
}
.form-check-input:active {
  background: #7749f8;
}
.form-control:focus,
.form-check-input:focus {
  border-color: #abb5be;
  box-shadow: none;
}

.search-bar {
  margin-bottom: 24px;
}
.dropdown-toggle::after {
  content: url(../assets/svg/downArrow.svg);
  border: none;
}
</style>
