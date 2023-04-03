<script setup lang="ts">
import { ref, withDefaults } from "vue";
const interfaceBlock = ref(false);
const routingBlock = ref(false);
const srpolicyBlock = ref(false);

interface Props {
  deviceInfo: {
    // general infomation
    asNumber: string;
    rd: string;
    importPolicy?: string;
    exportPolicy?: string;
    // interface
    interface: {
      name: string;
      description: string;
      ipAddress: {
        address: string;
        isSecondary: boolean;
      }[];
    }[];
    // routing
    bgpNeighbor: {
      address: string;
      as: string;
    }[];
    // srPolicy
    srPolicy?: {
      endpoint: string;
      color: string;
    }[];
  } | null;
}

withDefaults(defineProps<Props>(), {
  deviceInfo: {
    asNumber: "65534",
    rd: "65534:100",
    exportPolicy: "SET-C10",
    interface: [
      {
        name: "bundleEther1.100",
        description: "to-Node11",
        ipAddress: [{ address: "192.168.21.1/24", isSecondary: false }],
      },
      {
        name: "gigabitEthernet0/0/0/0",
        description: "to - Node21",
        ipAddress: [
          { address: "192.168.21.1/24", isSecondary: false },
          { address: "192.168.22.1/29", isSecondary: true },
          { address: "192.168.23.1/29", isSecondary: true },
        ],
      },
    ],
    // routing
    bgpNeighbor: [
      { address: "192.168.21.2", as: "18000" },
      { address: "192.168.21.3", as: "18000" },
    ],
    // 値（srPolicy）が無ければ非表示
    srPolicy: [
      { endpoint: "ACR11", color: "10" },
      { endpoint: "ACR12", color: "10" },
      { endpoint: "WSE12", color: "10" },
    ],
  },
});

// withDefaults(
//     defineProps<{
//       deviceInfo:
//         | {
//           // general infomation
//             asNumber: string
//             rd: string
//             importPolicy?: string
//             exportPolicy?: string
//             // interface
//             interface: {
//               name: string
//               description: string
//               ipAddress: {
//                 address: string
//                 isSecondary: boolean
//               }[]
//             }[]
//             // routing
//             bgpNeighbor: {
//               address: string
//               as: string
//             }[]
//             // srPolicy
//             srPolicy?: {
//               endpoint: string
//               color:string
//             }[]
//           }
//         | null
//     }>(),
//     {
//       deviceInfo: null,
//     }
//   )
</script>

<template>
  <v-card class="mx-auto" style="width: 500px">
    <!-- General Infomation start -->
    <v-card-title> General Infomation </v-card-title>

    <v-table fixed-header style="padding-left: 16px; padding-right: 16px">
      <tbody>
        <tr>
          <td>AS Number</td>
          <td>{{ deviceInfo?.asNumber }}</td>
        </tr>
        <v-divider style="border-top-width: 0px"></v-divider>

        <tr>
          <td>RD</td>
          <td>{{ deviceInfo?.rd }}</td>
        </tr>
        <v-divider style="border-top-width: 0px"></v-divider>

        <tr>
          <td>Import Policy</td>
          <td v-if="deviceInfo?.importPolicy">
            {{ deviceInfo?.importPolicy }}
          </td>
          <td v-else>-</td>
        </tr>
        <v-divider style="border-top-width: 0px"></v-divider>

        <tr>
          <td>Export Policy</td>
          <td v-if="deviceInfo?.exportPolicy">
            {{ deviceInfo?.exportPolicy }}
          </td>
          <td v-else>-</td>
        </tr>
      </tbody>
    </v-table>
    <!-- General Infomation end -->

    <!-- Interface start -->
    <v-card-actions style="padding: 0px">
      <v-card-title> Interface </v-card-title>
      <v-spacer></v-spacer>

      <v-btn
        :icon="interfaceBlock ? 'mdi-chevron-up' : 'mdi-chevron-down'"
        @click="interfaceBlock = !interfaceBlock"
      >
      </v-btn>
    </v-card-actions>

    <v-card elevation="0" style="padding-left: 16px; padding-right: 16px">
      <div v-show="interfaceBlock">
        <div
          v-for="(inter, interIndex) in deviceInfo?.interface"
          :key="interIndex"
        >
          <v-card-title> {{ inter.name }} </v-card-title>

          <v-table fixed-header style="padding-left: 16px; padding-right: 16px">
            <tbody>
              <tr>
                <td>Description</td>
                <td>{{ inter.description }}</td>
              </tr>
              <v-divider style="border-top-width: 0px"></v-divider>
              <tr>
                <td>IP Address</td>
                <td>
                  <div
                    v-for="(ipAddress, ipAddressIndex) in inter.ipAddress"
                    :key="ipAddressIndex"
                  >
                    {{ ipAddress.address }}
                    <span v-if="ipAddress.isSecondary"> secondary </span>
                  </div>
                </td>
              </tr>
            </tbody>
          </v-table>
        </div>
      </div>
    </v-card>
    <!-- Interface end -->

    <!-- Routing start -->
    <v-card-actions style="padding: 0px">
      <v-card-title> Routing </v-card-title>

      <v-spacer></v-spacer>

      <v-btn
        :icon="routingBlock ? 'mdi-chevron-up' : 'mdi-chevron-down'"
        @click="routingBlock = !routingBlock"
      >
      </v-btn>
    </v-card-actions>

    <div v-show="routingBlock">
      <div elevation="0" style="padding-left: 16px; padding-right: 16px">
        <v-card-title> BGP Neighbor </v-card-title>
        <v-table fixed-header style="padding-left: 16px; padding-right: 16px">
          <tbody>
            <tr
              v-for="(routing, routingIndex) in deviceInfo?.bgpNeighbor"
              :key="routingIndex"
            >
              <td>{{ routing.address }}</td>
              <td>AS {{ routing.as }}</td>
            </tr>
            <v-divider style="border-top-width: 0px"></v-divider>
          </tbody>
        </v-table>
      </div>
    </div>
    <!-- Routing end -->

    <!-- SR Policy start -->
    <div v-if="deviceInfo?.srPolicy">
      <v-card-actions style="padding: 0px">
        <v-card-title> SR Policy </v-card-title>
        <v-spacer></v-spacer>

        <v-btn
          :icon="srpolicyBlock ? 'mdi-chevron-up' : 'mdi-chevron-down'"
          @click="srpolicyBlock = !srpolicyBlock"
        >
        </v-btn>
      </v-card-actions>

      <div v-show="srpolicyBlock">
        <div elevation="0" style="padding-left: 16px; padding-right: 16px">
          <v-table fixed-header style="padding-left: 16px; padding-right: 16px">
            <thead>
              <tr>
                <th class="text-left">SR</th>
                <th class="text-left">Color</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(srpolicy, srpolicyIndex) in deviceInfo?.srPolicy"
                :key="srpolicyIndex"
              >
                <td>{{ srpolicy.endpoint }}</td>
                <td>{{ srpolicy.color }}</td>
              </tr>
              <v-divider style="border-top-width: 0px"></v-divider>
            </tbody>
          </v-table>
        </div>
      </div>
    </div>
    <div v-else="false"></div>
    <!-- SR Policy end -->
  </v-card>
</template>
