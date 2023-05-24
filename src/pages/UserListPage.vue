<template>
  <div class="user-list-container">
    <Button @click="handleButtonClick">Assign</Button>
    <UserList
      v-if="isButtonClicked"
      :enableSearch="true"
      :class="isUerListShow"
      :list="userListData"
      searchPlaceholder="Search User..."
      captions="Assign"
      @onAssign="handleAssignUser"
      @onSearch="onSearchHandler"
    />
  </div>
</template>

<script lang="ts">
/**
 *  Components Imports
 */
import { ref, defineComponent, computed } from 'vue'
import Button from '../components/Button.vue'
import UserList from '../components/UserList.vue'

/**
 *  Dummy User List
 */
const dummyUserList: IuserList[] = [
  {
    id: 1,
    name: 'Andrei Rus',
    email: 'Andrei.rus@gmail.com'
  },
  {
    id: 2,
    name: 'Florin',
    email: 'florin.@ing.ro'
  },
  {
    id: 3,
    name: 'Ioana Popescu',
    email: 'ioana.popescu@gmail.com'
  }
]

/**
 *  Types
 */

export interface IuserList {
  id: number
  name: string
  email: string
  isSelected?: boolean
}

export default defineComponent({
  setup() {
    /**
     *  Defined Ref are Refs
     */
    const isButtonClicked = ref<boolean>(false)
    const userListData = ref<IuserList[]>(dummyUserList)

    /**
     *  Methods
     */

    /**
     *  Toggle User Popover
     */
    const handleButtonClick = () => {
      isButtonClicked.value = !isButtonClicked.value
    }

    /**
     * User Select Method
     * @param userId
     */
    const handleAssignUser = (userId: number) => {
      const updatedUserList: IuserList[] = userListData.value.map((user: IuserList) => {
        if (user?.id === userId) {
          user['isSelected'] = user['isSelected'] ? !user['isSelected'] : true
        }
        return user
      })
      userListData.value = updatedUserList
    }

    /**
     * User Search Method
     * @param searchValue
     */
    const onSearchHandler = (searchValue: string) => {
      if (searchValue.length) {
        userListData.value = [...dummyUserList].filter(
          (user: IuserList) =>
            user.name.toLocaleLowerCase().includes(searchValue) ||
            user.email.toLocaleUpperCase().includes(searchValue)
        )
      } else {
        userListData.value = [...dummyUserList]
      }
    }

    /**
     *  Computed Function
     */
    const isUerListShow = computed(() => (isButtonClicked.value ? 'show' : ''))

    return {
      handleButtonClick,
      isButtonClicked,
      isUerListShow,
      userListData,
      handleAssignUser,
      onSearchHandler
    }
  },
  components: {
    Button,
    UserList
  }
})
</script>

<style scoped>
.user-list-container {
  display: flex;
  justify-content: center;
}
.btn {
  width: 126px;
  border: 1px solid #dee2e6;
  border-radius: 6px;
  align-items: center;
  padding: 9px 16px;
}
.btn:hover {
  border: 1px solid #dee2e6;
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
  transform: translate(-132px, 52px) !important;
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
  /* border: 2px solid #5227cc; */
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
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
