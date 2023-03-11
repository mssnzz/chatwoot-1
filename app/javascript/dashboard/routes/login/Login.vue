<template>
  <div class="medium-12 column login">
    <div class="text-center medium-12 login__hero align-self-top" style="margin-top: 5%!important;">
      <img
        src="https://beautiful-curran.146-190-54-185.plesk.page/openchat/openchatlogo.png"
        :alt="globalConfig.installationName" style="width: 250px!important;"
        class="hero__logo"
      />
      <h2 class="hero__title" style="color: #000!important; font-weight: 800; font-family: 'sfproblack'!important;">
        Te damos la bienvenida
      </h2>
      <p style=" color: #000!important;
          font-weight: 500;
          margin-top: 0%; font-size: 18px; letter-spacing: 0.3px;
      ">Ingresa tus datos para poder iniciar en el board</p>
    </div>
    <div class="row align-center">
      <div v-if="!email" class="small-12 medium-4 column">
        <form class="login-box column align-self-top" @submit.prevent="login()" style="box-shadow: none!important; border-width: 0px!important;">
          <div class="column log-in-form">
            <label :class="{ error: $v.credentials.email.$error }" style="color: #000!important; letter-spacing: 0.5px!important;">
              Correo electrónico
              <input
                v-model.trim="credentials.email"
                type="text"
                data-testid="email_input"
                placeholder="nombre@empresa.com" style="border-width: 2px!important; border-color: #000!important;"
                @input="$v.credentials.email.$touch"
              />
            </label>
            <label :class="{ error: $v.credentials.password.$error }" style="color: #000!important; letter-spacing: 0.5px!important;">
              Contraseña
              <input
                v-model.trim="credentials.password"
                type="password"
                data-testid="password_input"
                placeholder="contraseña" style="border-width: 2px!important; border-color: #000!important;"
                @input="$v.credentials.password.$touch"
              />
            </label>
            <woot-submit-button
              :disabled="
                $v.credentials.email.$invalid ||
                  $v.credentials.password.$invalid ||
                  loginApi.showLoading
              "
              button-text="Iniciar sesión"
              :loading="loginApi.showLoading"
              button-class="large expanded" style="letter-spacing: 0.5px!important;"
            />
          </div>
        </form>
        <div class="column text-center sigin__footer">
          <p v-if="!globalConfig.disableUserProfileUpdate" style="font-family: 'sfmedium'!important;">
            <router-link to="auth/reset/password">
              ¿Olvidaste tú contraseña?
            </router-link>
          </p>
          <p v-if="showSignupLink()">
            <router-link to="auth/signup">
              {{ $t('LOGIN.CREATE_NEW_ACCOUNT') }}
            </router-link>
          </p>
          <p>
            Con la tecnología de <a href="https://opensoftdo.com/">Opensoft Dominicana</a>
          </p>
        </div>
      </div>
      <woot-spinner v-else size="" />
    </div>
  </div>
</template>

<style>
@font-face {
  font-family: 'sfregular';
  src: url('../../assets/font/sfpro-regular.ttf');
}
@font-face {
  font-family: 'sfmedium';
  src: url('../../assets/font/sfpro-medium.ttf');
}
@font-face {
  font-family: 'sfproblack';
  src: url('../../assets/font/sfpro-black.ttf');
}
.newFont, p, label, .expanded, input, woot-submit-button, .button, .primary, .link{
  font-family: 'biperegular'!important;
}

</style>

<script>
import { required, email } from 'vuelidate/lib/validators';
import globalConfigMixin from 'shared/mixins/globalConfigMixin';
import WootSubmitButton from '../../components/buttons/FormSubmitButton';
import { mapGetters } from 'vuex';

export default {
  components: {
    WootSubmitButton,
  },
  mixins: [globalConfigMixin],
  props: {
    ssoAuthToken: { type: String, default: '' },
    ssoAccountId: { type: String, default: '' },
    ssoConversationId: { type: String, default: '' },
    config: { type: String, default: '' },
    email: { type: String, default: '' },
  },
  data() {
    return {
      // We need to initialize the component with any
      // properties that will be used in it
      credentials: {
        email: '',
        password: '',
      },
      loginApi: {
        message: '',
        showLoading: false,
      },
      error: '',
    };
  },
  validations: {
    credentials: {
      password: {
        required,
      },
      email: {
        required,
        email,
      },
    },
  },
  computed: {
    ...mapGetters({
      globalConfig: 'globalConfig/get',
    }),
  },
  created() {
    if (this.ssoAuthToken) {
      this.login();
    }
  },
  methods: {
    showAlert(message) {
      // Reset loading, current selected agent
      this.loginApi.showLoading = false;
      this.loginApi.message = message;
      bus.$emit('newToastMessage', this.loginApi.message);
    },
    showSignupLink() {
      return window.chatwootConfig.signupEnabled === 'true';
    },
    login() {
      this.loginApi.showLoading = true;
      const credentials = {
        email: this.email
          ? decodeURIComponent(this.email)
          : this.credentials.email,
        password: this.credentials.password,
        sso_auth_token: this.ssoAuthToken,
        ssoAccountId: this.ssoAccountId,
        ssoConversationId: this.ssoConversationId,
      };
      this.$store
        .dispatch('login', credentials)
        .then(() => {
          this.showAlert(this.$t('LOGIN.API.SUCCESS_MESSAGE'));
        })
        .catch(response => {
          // Reset URL Params if the authentication is invalid
          if (this.email) {
            window.location = '/app/login';
          }

          if (response && response.status === 401) {
            const { errors } = response.data;
            const hasAuthErrorMsg =
              errors &&
              errors.length &&
              errors[0] &&
              typeof errors[0] === 'string';
            if (hasAuthErrorMsg) {
              this.showAlert(errors[0]);
            } else {
              this.showAlert(this.$t('LOGIN.API.UNAUTH'));
            }
            return;
          }
          this.showAlert(this.$t('LOGIN.API.ERROR_MESSAGE'));
        });
    },
  },
};
</script>
