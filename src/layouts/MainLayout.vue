<template>
  <q-layout view="lHh Lpr lFf">
    <q-header bordered class="bg-red">
      <q-toolbar>
        <q-avatar>
          <img
            @click="$router.push('/').catch(() => {})"
            src="https://firebasestorage.googleapis.com/v0/b/bagn-chick.appspot.com/o/bnc.jpg?alt=media&token=dec41778-5060-4e88-b3d8-84061f0ab69e"
            />
        </q-avatar>
        <q-toolbar-title @click="$router.push('/').catch(() => {})">
          {{$route.name}}
        </q-toolbar-title>

        <q-btn
          flat
          v-if="!$q.localStorage.getItem('user').isAdmin"
          dense
          to="basket"
          color="white"
          @click="layoutNotification = true"
          round
          aria-label="Menu"
          icon="shopping_basket"
        >
          <q-badge v-if="carts.length !== 0" color="white" text-color="red" floating transparent>
            {{carts.length}}
          </q-badge>
        </q-btn>
  
        <q-btn
          flat
          dense
          v-if="$q.localStorage.getItem('user').isAdmin"
          to="order_history"
          color="white"
          round
          aria-label="Menu"
          icon="pending_actions"
        >
          <q-badge v-if="orders.length !== 0" color="white" text-color="red" floating transparent>
            {{orders.length}}
          </q-badge>
        </q-btn>
      </q-toolbar>
    </q-header>

    <q-footer bordered class="bg-white text-red-4">
      <q-tabs indicator-color="transparent" align="justify" dense no-caps active-color="red">
        <q-route-tab
          icon="home"
          label="Home"
          to="/"
          exact
        />
        <q-route-tab
          v-if="$q.localStorage.getItem('user').isAdmin"
          icon="dashboard"
          label="Dashboard"
          to="/dashboard"
          exact
        />
        <q-route-tab
          icon="history"
          label="Order History"
          to="/order_history"
          exact
        />
        <q-route-tab
          icon="person"
          label="Account"
          to="/account"
          exact
        />
      </q-tabs>
    </q-footer>

    <q-page-container>
      <router-view :carts="carts" />
    </q-page-container>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout',
  data () {
    return {
      carts: [],
      orders: []
    }
  },
  mounted () {
    if (this.$q.localStorage.getItem('user')) {
      this.$bind('carts', this.$db.collection('carts').where('user.uid', '==',  this.$q.localStorage.getItem('user').uid)).then(carts => {
        this.carts = carts
      })
    }

    this.$bind('orders', this.$db.collection('orders').where('status', '==', 'Pending')).then(orders => {
      this.orders = orders
    })
  }
}
</script>
