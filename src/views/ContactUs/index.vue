<template>
  <div class="contact-us bg-seas-hell mx-auto">
    <h1 class="text-center mb-4">{{ $t('page.contactUs') }}</h1>
    <organism-form @on-submit="handleSubmit">
      <div class="row">
        <div
          class="col-md-12 col-sm-12"
          v-for="({ value, ...rest }, index) in fields"
          :key="`contact-us-field-${index}`"
        >
          <molecule-input :name="$t(`contactUs.${value}`)" v-bind="rest" v-model="message[value]" />
        </div>
      </div>
      <atom-button flat block class="my-4" type="submit">{{ $t('action.send') }}</atom-button>
    </organism-form>
  </div>
</template>

<script>
export default {
  name: 'ContactUs',
  data() {
    return {
      message: {},
    };
  },
  methods: {
    handleSubmit({ isValid: { valid } } = {}) {
      if (valid) {
        this.$store.commit('contactUs/setCotactUsMessage', {
          message: this.message,
        });
        this.$router.push('/');
      }
    },
  },
  computed: {
    fields() {
      return [
        { validate: 'required', value: 'title' },
        { validate: 'required', value: 'name' },
        { validate: 'required|email', value: 'email' },
        { validate: 'required', value: 'country' },
        { type: 'textarea', validate: 'required|min:20|max:100', value: 'message' },
      ];
    },
    userInfo() {
      return this.$store.getters['auth/getUser'];
    },
  },
};
</script>
