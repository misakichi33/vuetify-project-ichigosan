<script setup lang="ts">
  // import DefaultBar from './AppBar.vue'
  import DefaultView from './View.vue'
  // import SideBar from '@/components/DeviceInfoComponent.vue'
  import DeviceInfoComponent from '@/components/DeviceInfoComponent.vue';

  // const props = withDefaults(defineProps<Props>(), {
  //   asNumber: '65534',
  //   rd: '65534:100',
  //   exportPolicy: 'SET-C10',
  //   interface: [
  //     {
  //       name: 'bundleEther1.100',
  //       description: 'to-Node11',
  //       ipAddress: [{ address: '192.168.21.1/24', isSecondary: false }],
  //     },
  //     {
  //       name: 'gigabitEthernet0/0/0/0',
  //       description: 'to - Node21',
  //       ipAddress: [
  //         { address: '192.168.21.1/24', isSecondary: false },
  //         { address: '192.168.22.1/29', isSecondary: true },
  //         { address: '192.168.23.1/29', isSecondary: true },
  //       ],
  //     },
  //   ],
  //   // routing
  //   bgpNeighbor: [
  //     { address: '192.168.21.2', as: '18000' },
  //     { address: '192.168.21.3', as: '18000' },
  //   ],
  //   // 値（srPolicy）が無ければ非表示
  //   srPolicy: [
  //     { endpoint: 'ACR11', color: '10' },
  //     { endpoint: 'ACR12', color: '10' },
  //     { endpoint: 'WSE12', color: '10' }
  //     ],
  // }
  // )
</script>

<template>
  <v-app>
    <!-- <default-bar /> -->
    <DeviceInfoComponent :device-info="deviceInfo"/>
    <default-view />
  </v-app>
</template>