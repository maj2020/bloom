<template>
  <div>
    <v-hover v-slot:default="{ hover }">
      <v-avatar :class="{'blm-pointer': enabled }" size="60" @click="openAvatarUploadDialog">
        <v-img :src="avatarUrl" >
          <v-expand-transition>
            <div
              v-if="hover && enabled"
  class="d-flex transition-fast-in-fast-out grey darken-4 blm-avatar-reveal white--text blm-pointer"
              style="height: 100%;"
            >
              Update
            </div>
          </v-expand-transition>
        </v-img>
      </v-avatar>
    </v-hover>
    <v-progress-circular
      :size="60"
      color="primary"
      indeterminate
      v-if="loading"
    />

    <input type="file" class="blm-avatar-upload" ref="blmavatarupload"
      v-on:change="updateAvatar()"/>
  </div>
</template>


<script lang="ts">
import { VueApp } from '@/app/vue';
import { PropType } from 'vue';

export default VueApp.extend({
  name: 'BAvatarForm',
  props: {
    avatarUrl: {
      type: String as PropType<string>,
      required: true,
    },
    loading: {
      type: Boolean as PropType<boolean>,
      required: false,
      default: false,
    },
    disabled: {
      type: Boolean as PropType<boolean>,
      required: false,
      default: false,
    },
  },
  computed: {
    enabled(): boolean {
      return !this.disabled;
    },
  },
  methods: {
    openAvatarUploadDialog() {
      if (this.disabled) {
        return;
      }

      const upload = this.$refs.blmavatarupload as HTMLElement;
      upload.click();
    },
    updateAvatar() {
      // eslint-disable-next-line @typescript-eslint/no-explicit-any
      const upload = this.$refs.blmavatarupload as any; // ugly
      if (upload.files.length !== 1) {
        return;
      }
      const file = upload.files[0];
      this.$emit('update-avatar', file);
    },
  },
});
</script>


<style lang="scss" scoped>
.blm-avatar-reveal {
  align-items: center;
  bottom: 0;
  justify-content: center;
  opacity: .5;
  position: absolute;
  width: 100%;
  cursor: pointer;
}

.blm-avatar-upload {
  display: none;
}
</style>
