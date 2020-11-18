<template>
  <div class="sb-notification" :class="returnClasses">
    <SbBadge
      class="sb-notification--icon-container"
      only-icon
      :type="returnTypeOfBadge"
    />
    <span class="sb-notification--title">
      {{ title }}
    </span>

    <button class="sb-notification--btn" @click="handleAction">
      <SbIcon :name="returnIconName" size="small" />
    </button>

    <SbNotificationDescription v-if="returnShowData">
      {{ description }}
    </SbNotificationDescription>

    <SbLink
      v-if="returnShowData"
      class="sb-notification--link"
      :label="linkName"
      :href="link"
      :title="linkName"
    />
  </div>
</template>

<script>
import SbLink from '../Link'
import SbBadge from '../Badge'
import SbIcon from '../Icon'
import SbNotificationDescription from './components/NotificationDescription'

export default {
  name: 'SbNotification',

  components: {
    SbBadge,
    SbIcon,
    SbNotificationDescription,
    SbLink,
  },

  props: {
    status: {
      type: String,
      default: 'general',
    },
    title: {
      type: String,
      default: 'notification',
    },
    description: {
      type: String,
      default: null,
    },
    link: {
      type: String,
      default: null,
    },
    linkName: {
      type: String,
      default: null,
    },
    isExpandable: {
      type: Boolean,
      default: false,
    },
    isFull: {
      type: Boolean,
      default: false,
    },
  },

  data() {
    return {
      expandle: false,
    }
  },

  computed: {
    returnClasses() {
      const classes = {
        'sb-notification--expandable': this.expandle,
        'sb-notification--full': this.isFull,
        'sb-notification--content':
          !this.isFull && !this.isExpandable && (this.description || this.link),
        'sb-notification--full-content':
          this.isFull && (this.description || this.link) && !this.isExpandable,
      }

      return [classes, `sb-notification--${this.status}`]
    },

    returnTypeOfBadge() {
      return this.status === 'general' ? 'info' : this.status
    },

    returnIconName() {
      return this.isExpandable && !this.expandle
        ? 'chevron-down'
        : this.isExpandable && this.expandle
        ? 'chevron-up'
        : 'close'
    },

    returnShowData() {
      return this.isExpandable && !this.expandle
        ? false
        : !!(this.isExpandable && this.expandle)
    },
  },

  methods: {
    expandleNotification() {
      this.expandle = !this.expandle
      if (this.expandle) {
        this.$emit('expandle')
        return
      }

      this.$emit('retract')
    },

    closeNotification() {
      this.$emit('hide')
    },

    handleAction() {
      if (this.isExpandable) {
        this.expandleNotification()
        return
      }

      this.closeNotification()
    },
  },
}
</script>
