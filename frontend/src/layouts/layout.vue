<template>
  <q-layout view="hHh LpR ffr">
    <q-header elevated class="bg-primary">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          @click="leftDrawerOpen = !leftDrawerOpen"
          icon="menu"
          aria-label="Menu"
        />

        <q-toolbar-title>
          Умный дом
        </q-toolbar-title>

        <q-btn flat :label="getCurrentUser" @click="profileButton" />

        <q-btn
          flat
          round
          dense
          icon="fas fa-sign-out-alt"
          @click="logoutButton"
          aria-label="Menu"
        />
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header>Возможности</q-item-label>

        <q-item clickable to="/addNew" v-if="getRights[1] === 'true'">
          <q-item-section avatar>
            <q-icon name="add" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Добавить устройство</q-item-label>
            <q-item-label caption>Добавить новое устройство</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable to="/list" v-if="getRights[0] === 'true'">
          <q-item-section avatar>
            <q-icon name="list" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Вывести устройства</q-item-label>
            <q-item-label caption>Вывести список всех устройств</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable to="/delete" v-if="getRights[1] === 'true'">
          <q-item-section avatar>
            <q-icon name="delete" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Удалить устройство</q-item-label>
            <q-item-label caption>Удалить устройство из системы</q-item-label>
          </q-item-section>
        </q-item>

        <!---->

        <q-item-label header v-if="getRights[3] === 'true'"
          >Сырой вывод</q-item-label
        >

        <q-item clickable to="/rawAreas" v-if="getRights[3] === 'true'">
          <q-item-section avatar>
            <q-icon name="map" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Пространства</q-item-label>
            <q-item-label caption>Таблица "Пространства"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable to="/rawCommands" v-if="getRights[3] === 'true'">
          <q-item-section avatar>
            <q-icon name="contactless" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Команды</q-item-label>
            <q-item-label caption>Таблица "Команды"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item
          clickable
          to="/rawCommandsByDevice"
          v-if="getRights[3] === 'true'"
        >
          <q-item-section avatar>
            <q-icon name="device_hub" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Управление</q-item-label>
            <q-item-label caption>Таблица "Управление"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable to="/rawCommandTypes" v-if="getRights[3] === 'true'">
          <q-item-section avatar>
            <q-icon name="contactless" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Типы команд</q-item-label>
            <q-item-label caption>Таблица "Типы команд"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable to="/rawDevices" v-if="getRights[3] === 'true'">
          <q-item-section avatar>
            <q-icon name="devices_other" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Устройства</q-item-label>
            <q-item-label caption>Таблица "Устройства"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item
          clickable
          to="/rawDeviceParameters"
          v-if="getRights[3] === 'true'"
        >
          <q-item-section avatar>
            <q-icon name="perm_device_information" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Параметры устройств</q-item-label>
            <q-item-label caption>Таблица "Параметры устройств"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable to="/rawDeviceTypes" v-if="getRights[3] === 'true'">
          <q-item-section avatar>
            <q-icon name="devices_other" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Типы устройств</q-item-label>
            <q-item-label caption>Таблица "Типы устройств"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item
          clickable
          to="/rawDevicesByUserAccount"
          v-if="getRights[3] === 'true'"
        >
          <q-item-section avatar>
            <q-icon name="device_hub" />
          </q-item-section>
          <q-item-section>
            <q-item-label>
              Контролируемые учетной записью устройства
            </q-item-label>
            <q-item-label caption>
              Таблица "Контролируемые учетной записью устройства"
            </q-item-label>
          </q-item-section>
        </q-item>

        <q-item
          clickable
          to="/rawDevicesInProfile"
          v-if="getRights[3] === 'true'"
        >
          <q-item-section avatar>
            <q-icon name="device_hub" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Содержащиеся в профиле устройства</q-item-label>
            <q-item-label caption
              >Таблица "Содержащиеся в профиле устройства"</q-item-label
            >
          </q-item-section>
        </q-item>

        <q-item clickable to="/rawProfiles" v-if="getRights[3] === 'true'">
          <q-item-section avatar>
            <q-icon name="person_outline" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Профили</q-item-label>
            <q-item-label caption>Таблица "Профили"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable to="/rawRealEstates" v-if="getRights[3] === 'true'">
          <q-item-section avatar>
            <q-icon name="king_bed" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Помещения</q-item-label>
            <q-item-label caption>Таблица "Помещения"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item
          clickable
          to="/rawRealEstateTypes"
          v-if="getRights[3] === 'true'"
        >
          <q-item-section avatar>
            <q-icon name="location_city" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Типы помещений</q-item-label>
            <q-item-label caption>Таблица "Типы помещений"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item clickable to="/rawUserAccounts" v-if="getRights[3] === 'true'">
          <q-item-section avatar>
            <q-icon name="people" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Пользователи</q-item-label>
            <q-item-label caption>Таблица "Пользователи"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item
          clickable
          to="/rawUserAccountTypes"
          v-if="getRights[3] === 'true'"
        >
          <q-item-section avatar>
            <q-icon name="group" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Типы пользователей</q-item-label>
            <q-item-label caption>Таблица "Типы пользователей"</q-item-label>
          </q-item-section>
        </q-item>

        <q-item
          clickable
          to="/rawUserAccountRights"
          v-if="getRights[3] === 'true'"
        >
          <q-item-section avatar>
            <q-icon name="border_clear" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Права пользователей</q-item-label>
            <q-item-label caption>Таблица "Права пользователей"</q-item-label>
          </q-item-section>
        </q-item>

        <!---->

        <q-item-label header>Настройки</q-item-label>

        <q-item clickable to="/log" v-if="getRights[5] === 'true'">
          <q-item-section avatar>
            <q-icon name="history" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Журнал</q-item-label>
            <q-item-label caption
              >Журнал событий информационной системы</q-item-label
            >
          </q-item-section>
        </q-item>

        <q-item clickable to="/about" v-if="getRights[0] === 'true'">
          <q-item-section avatar>
            <q-icon name="info" />
          </q-item-section>
          <q-item-section>
            <q-item-label>Справка</q-item-label>
            <q-item-label caption>Информация о проекте</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { addLogRecord } from "../lib/log";

export default {
  name: "Layout",

  data() {
    return {
      leftDrawerOpen: false
    };
  },

  computed: {
    getCurrentUser() {
      return localStorage.getItem("username");
    },

    getRights() {
      return localStorage.getItem("rights").split(",");
    }
  },

  methods: {
    profileButton() {
      this.$router.push("/profile");
    },

    logoutButton() {
      addLogRecord({
        log_level: "info",
        message: `Пользователь с именем ${localStorage.getItem(
          "username"
        )} вышел из системы`
      });

      this.$store.dispatch("logout");
      this.$router.push("/auth");
    }
  }
};
</script>
