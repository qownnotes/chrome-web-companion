<template>
  <v-dialog :value="value" max-width="500px" @input="$emit('input')">
    <v-card>
      <v-card-title>
        <span class="headline">{{ getLocale('InputTokenHeadline') }}</span>
      </v-card-title>

      <v-card-text>
        <v-container grid-list-md>
          <v-layout wrap>
            <v-flex sm12 xs12>
              {{ getLocale('InputToken') }}
            </v-flex>
            <v-flex sm12 xs12>
              <v-text-field
                  ref="tokenText"
                  v-model="token"
                  :label="getLocale('Token')"
                  clearable
                  required
              ></v-text-field>
            </v-flex>
          </v-layout>
        </v-container>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click.native="$emit('input')">{{ getLocale('Cancel') }}</v-btn>
        <v-btn color="blue darken-1" text @click="storeToken">{{ getLocale('Ok') }}</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>

</template>

<script>
import * as util from '../scripts/util';

export default {
  name: "InputTokenDialog",
  props: ['webSocket', 'value'],
  data() {
    return {
      token: ''
    }
  },
  mounted() {
    let that = this;

    chrome.storage.sync.get(['token'], function (result) {
      that.token = result.token;
      // Todo: Doesn't seem to do anything
      // that.$nextTick(() => that.$refs.tokenText.focus());
    });
  },
  methods: {
    getLocale(text) {
      return util.getLocale(text);
    },
    storeToken() {
      let that = this;

      chrome.storage.sync.set({
        token: this.token
      }, function () {
        that.$emit('input');
      });
    }
  }
}
</script>

<style scoped>

</style>