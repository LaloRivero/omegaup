<template>
  <div class="panel panel-primary" v-if="requests.length != 0">
    <div class="panel-body">
      {{ T.pendingRegistrations }}
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>{{ T.wordsUser }}</th>
          <th>{{ T.userEditCountry }}</th>
          <th>{{ T.requestDate }}</th>
          <th>{{ T.currentStatus }}</th>
          <th>{{ T.lastUpdate }}</th>
          <th>{{ T.contestAdduserAddContestant }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="request in requests">
          <td>{{ request.username }}</td>
          <td>{{ request.country }}</td>
          <td>{{ request.request_time }}</td>
          <td v-if="request.last_update == null">{{ T.wordsPending }}</td>
          <td v-else-if="request.accepted == 'true' || request.accepted == '1'">
            {{ T.wordAccepted }}
          </td>
          <td v-else="">{{ T.wordsDenied }}</td>
          <td v-if="request.last_update != null">
            {{ request.last_update }} ({{ request.admin.username }})
          </td>
          <td v-else=""></td>
          <td
            v-if="request.accepted != 'true' &amp;&amp; request.accepted != '1'"
          >
            <button
              class="close"
              style="color:red"
              v-on:click="onDenyRequest(request.username)"
            >
              ×
            </button>
            <button
              class="close"
              style="color:green"
              v-on:click="onAcceptRequest(request.username)"
            >
              ✓
            </button>
          </td>
          <td v-else=""></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import { T } from '../../omegaup.js';
import omegaup from '../../api.js';

@Component({})
export default class Requests extends Vue {
  @Prop() data!: omegaup.IdentityContestRequest[];

  T = T;
  requests = this.data;

  onAcceptRequest(username: string): void {
    this.$emit('emit-accept-request', this, username);
  }
  onDenyRequest(username: string): void {
    this.$emit('emit-deny-request', this, username);
  }
}
</script>
