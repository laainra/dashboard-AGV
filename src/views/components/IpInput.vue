<template>
    <div
      v-show="show"
      class="modal fade"
      :class="[
        { 'show d-block': show },
        { 'd-none': !show },
        { 'modal-mini': type === 'mini' },
      ]"
      tabindex="-1"
      role="dialog"
      :aria-hidden="!show"
      @mousedown.self="closeModal"
    >
      <div
        class="modal-dialog modal-dialog-centered"
        :class="[
          { 'modal-notice': type === 'notice', [`modal-${size}`]: size },
          modalClasses,
        ]"
      >
        <div
          class="modal-content"
          :class="[
            gradient ? `bg-gradient-${gradient}` : '',
            modalContentClasses,
          ]"
        >
          <div v-if="$slots.header" class="modal-header align-items-center" :class="[headerClasses]">
            <slot name="header"></slot>
            <slot name="close-button">
              <argon-button
                v-if="showClose"
                type="button"
                class="close"
                data-dismiss="modal"
                aria-label="Close"
                @click="closeModal"
              >
                <span :aria-hidden="!show">×</span>
              </argon-button> 
            </slot>
          </div>
  
          <div class="modal-body" :class="bodyClasses">
            <slot name="body"></slot>
          </div>
  
          <div v-if="$slots.footer" class="modal-footer" :class="footerClasses">
            <slot name="footer"></slot>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ip-input',
    props: {
      show: {
        type: Boolean,
        required: true,
      },
      showClose: {
        type: Boolean,
        default: true,
      },
      type: {
        type: String,
        default: '',
        validator(value) {
          const acceptedValues = ['', 'notice', 'mini'];
          return acceptedValues.indexOf(value) !== -1;
        },
        description: 'Modal type (notice|mini|"") ',
      },
      modalClasses: {
        type: [Object, String],
        description: 'Modal dialog css classes',
        default: '',
      },
      size: {
        type: String,
        description: 'Modal size',
        validator(value) {
          const acceptedValues = ['', 'sm', 'lg'];
          return acceptedValues.indexOf(value) !== -1;
        },
        default: '',
      },
      modalContentClasses: {
        type: [Object, String],
        description: 'Modal dialog content css classes',
        default: '',
      },
      gradient: {
        type: String,
        description: 'Modal gradient type (danger, primary etc)',
        default: '',
      },
      headerClasses: {
        type: [Object, String],
        description: 'Modal Header css classes',
        default: '',
      },
      bodyClasses: {
        type: [Object, String],
        description: 'Modal Body css classes',
        default: '',
      },
      footerClasses: {
        type: [Object, String],
        description: 'Modal Footer css classes',
        default: '',
      },
      animationDuration: {
        type: Number,
        default: 500,
        description: 'Modal transition duration',
      },
    },
    emits: ['update:show', 'close'],
    watch: {
      show(val) {
        const documentClasses = document.body.classList;
        if (val) {
          documentClasses.add('modal-open');
        } else {
          documentClasses.remove('modal-open');
        }
      },
    },
    methods: {
      closeModal() {
        this.$emit('update:show', false);
        this.$emit('close');
      },
    },
  };
  </script>
  <style>
  .modal.show {
    background-color: rgba(0, 0, 0, 0.3);
  }
  </style>
  