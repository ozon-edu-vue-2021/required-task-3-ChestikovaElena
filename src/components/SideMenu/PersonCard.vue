<template>
  <div class="person">
    <div class="person__content">
      <div class="person__photo">
        <img :src="person.picture" alt="photo" />
      </div>
      <div class="person__info-name">
        <b>{{ person.name }}</b>
      </div>
    </div>
    <div class="person__actions">
      <Icon
        iconType="home"
        :selectedIcon="selectedIcon" 
        @update:selectedIcon="selectIcon"
      />
      <Icon iconType="email"
        :selectedIcon="selectedIcon" 
        @update:selectedIcon="selectIcon"
      />
      <Icon iconType="more"
        :selectedIcon="selectedIcon" 
        @update:selectedIcon="selectIcon"
      />
      <Icon iconType="link"
        :selectedIcon="selectedIcon" 
        @update:selectedIcon="selectIcon"
      />
    </div>
      <div class="person__info">
        <div
          v-if="selectedIcon === 'home'"
          class="person__info-block"
        >
          <div class="info__title">Основная иформация</div>
          <div class="person__info-subdivision">
            {{ subdivision }}
          </div>
          <div class="person__info-email">
            Работает с {{ formatedDate }}
          </div>
          <div>Полных лет: {{ person.age }}</div>
        </div>
        <div
          v-if="selectedIcon === 'email'"
          class="person__info-block"
        >
          <div class="person__info-email">
            <span class="info__title">Почта: </span>
              {{ person.email }}
          </div>
        </div>
        <div
          v-if="selectedIcon === 'more'"
          class="person__info-block"
        >
          <div class="person__info-about">
            <span class="info__title">О себе: </span>
            {{ person.about }}
          </div>
        </div>
        <div
          v-if="selectedIcon === 'link'"
          class="person__info-block"
        >
          <div class="person__info-link">
            <span class="info__title">Рабочее место № {{ person.tableId }}</span>
          </div>
        </div>
      </div>
    
  </div>
</template>

<script>
import Icon from './Icon.vue';
import { format, parseISO } from 'date-fns';

export default {
  props: {
    person: {
      type: Object,
      default: null,
    },
    subdivision: {
      type: String,
      default: null,
    }
  },
  computed: {
    formatedDate() {
      const date = parseISO(this.person.registered);
      return format(date, 'MM.dd.yyyy');
    },
  },
  components: {
    Icon,
  },
  data() {
    return {
      selectedIcon: 'home',
    }
  },
  methods: {
    selectIcon(icon) {
      this.selectedIcon = icon;
    }
  }
};
</script>

<style scoped>
.person {
  display: flex;
  flex-direction: column;
}

.person__content {
  margin-bottom: 20px;
  display: grid;
  grid-template-columns: 50px 1fr;
  grid-gap: 16px;
  align-items: center;
}

.person__photo img {
  height: 50px;
  width: 50px;
  border-radius: 50%;
}

.person__actions {
  margin-bottom: 30px;
  display: flex;
  justify-content: space-between;
}

.person__info {
  margin-left: 10px;
  display: grid;
  grid-gap: 4px;
}

.person__info-name {
  color: royalblue;
  font-size: 18px;
}

.person__info-subdivision {
  color: grey;
  font-weight: 700;
}

.info__title {
  color: royalblue;
  font-weight: 700;
  margin-bottom: 10px;
}

</style>
