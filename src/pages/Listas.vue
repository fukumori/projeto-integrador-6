<template>
  <q-page class="q-px-lg q-pt-xl g-grey-3 column">
    <h5 class="q-mt-none">Listas</h5>
    <div class="wor q-pa-sm bg-accent">
      <q-input
        class="col"
        square
        filled
        bg-color="white"
        v-model="newList"
        @keyup.enter="addList"
        placeholder="Criar Lista"
        dense>
        <template v-slot:append>
          <q-btn
            @click="addList"
            round
            dense
            flat
            color="primary"
            icon="add" />
        </template>
      </q-input>
    </div>
    <q-list
      class="bg-white"
      separator
      bordered>
      <q-item
        v-for="(list, index) in lists"
        :key="list.title"
        clickable
        v-ripple>
        <q-item-section>
          <q-item-label>{{ list.title }}</q-item-label>
        </q-item-section>
        <q-item-section side>
          <q-btn
            @click.stop="deleteList(index)"
            flat
            round
            dense
            color="primary"
            icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div
      v-if="!lists.length"
      class="no-lists absolute-center text-center">
      <q-icon
        name="check"
        size="100px"
        color="primary" />
        <div class="text-h5 text-primary">Nenhuma Lista</div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newList: '',
      lists: [
      ]
    }
  },
  methods: {
    getLists() {
      this.$axios.get('http:///lists', {
        action: 'fetchAll'
      }).then(function (response) {
        application.lists = response.data;
      }).then((res) => {
        console.log('POSTS', res.data)
      });
    },
    addList() {
      if (this.newList) {
        this.lists.push({
          title: this.newList,
        })
        this.newList = ''
      }
    },
    deleteList(index) {
      this.$q.dialog({
        title: 'Confirmar',
        message: 'Deseja remover a lista?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.lists.splice(index, 1)
        this.$q.notify('Lista removido com sucesso')
      })
    }
  },
  created: {

  },
  mounted() {
    this.getLists()
  }

}
</script>
<style lang="scss">
  .no-lists {
    opacity: 0.5;
  }
</style>
