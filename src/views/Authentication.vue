<template>
  <div class="row">
    <amplify-authenticator
      class="authenticator__form"
      v-bind:authConfig="authConfig"
    ></amplify-authenticator>

    
  </div>
</template>

<script>
// @ts-ignore
import { AmplifyEventBus } from "aws-amplify-vue";

const authConfig = {
  signUpConfig: {
    defaultCountryCode: "1",
    header: "Sign up for a new account",
    hideAllDefaults: false,
    signUpFields: [
      {
        label: 'UserName',
        key: 'username',
        required: true,
        placeholder: 'UserName',
        type: 'string',
        displayOrder: 1
      },
      {
        label: 'First Name',
        key: 'given_name',
        required: true,
        placeholder: 'First Name',
        type: 'string',
        displayOrder: 2
      },
      {
        label: 'Last Name',
        key: 'family_name',
        required: true,
        placeholder: 'Last Name',
        type: 'string',
        displayOrder: 3
      },
      {
        label: 'Email',
        key: 'email',
        required: true,
        placeholder: 'Email',
        type: 'email',
        displayOrder: 4
      },
      {
        label: 'Phone',
        key: 'phone_number',
        required: true,
        placeholder: 'Phone',
        type: 'string',
        displayOrder: 5
      },
      {
        label: 'Password',
        key: 'password',
        required: true,
        placeholder: 'Password',
        type: 'password',
        displayOrder: 6
      }
    ]
  }
};

/**
 * Authentication view authenticates a customer and redirects to desired page if successful
 * Non-authenticated users are redirected to this view via Route Guards
 */
export default {
  name: "Authentication",
  /**
   * @param {string} redirectTo - Sets Route one must go once authenticated
   */
  props: {
    redirectTo: String
  },
  data() {
    return {
      authConfig
    }
  },
  mounted() {
    /**
     * At mount lifecycle hook, it listens for `authState` event, and when successfully signed-in it redirects to desired page
     */
    AmplifyEventBus.$on("authState", info => {
      if (info === "signedIn") {
        // return to where we came from
        this.$router.push({ name: this.redirectTo });
      }
    });
  }
};
</script>

<style lang="stylus">
@import '~variables'

:root
  // Not a safe way to override as this can change at build
  // https://github.com/aws-amplify/amplify-js/issues/2471
  --amazonOrange $secondary !important
  --color-primary $primary !important

.authenticator__form
  @media only screen and (min-device-width: 700px)
    margin auto
    padding 15vmin

  > *
    font-family 'Raleway', 'Open Sans', sans-serif

  @media only screen and (min-device-width: 300px) and (max-device-width: 700px)
    > div
      min-width 80vw
      padding 10vmin
</style>
