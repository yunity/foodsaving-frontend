<template>
  <form
    @submit.prevent="maybeSave"
  >
    <MarkdownInput
      v-model="edit.comment"
      class="q-mx-sm"
      :label="$t('ACTIVITY_FEEDBACK.COMMENT_PLACEHOLDER')"
      @keyup.ctrl.enter="maybeSave"
    />

    <AmountPicker
      v-if="hasWeight"
      v-model="edit.weight"
      class="q-ml-sm"
      style="margin-top: 40px"
    />

    <div
      v-if="hasWeight && hasMultipleParticipants"
      class="row no-wrap items-center q-mx-sm text-caption"
    >
      <QIcon
        name="info"
        size="1.5em"
        class="q-mr-sm text-grey"
      />
      <div v-t="'ACTIVITY_FEEDBACK.AMOUNT_INFO'" />
    </div>

    <div
      v-if="hasAnyError"
      class="text-negative"
      style="margin-top: 3em"
    >
      <i class="fas fa-exclamation-triangle" />
      {{ anyFirstError }}
    </div>

    <div class="row justify-end q-ma-md q-gutter-sm">
      <QBtn
        v-if="!isNew"
        type="button"
        :disable="!hasChanged"
        @click="reset"
      >
        {{ $t('BUTTON.RESET') }}
      </QBtn>
      <QBtn
        type="submit"
        color="secondary"
        :loading="isPending"
        :disable="!canSave"
      >
        <span v-t="isNew ? 'BUTTON.CREATE' : 'BUTTON.SAVE_CHANGES'" />
      </QBtn>
    </div>
  </form>
</template>

<script>
import {
  QBtn,
  QIcon,
} from 'quasar'
import AmountPicker from './AmountPicker'
import MarkdownInput from '@/utils/components/MarkdownInput'
import editMixin from '@/utils/mixins/editMixin'
import statusMixin from '@/utils/mixins/statusMixin'

export default {
  components: {
    QBtn,
    QIcon,
    AmountPicker,
    MarkdownInput,
  },
  mixins: [statusMixin, editMixin],
  props: {
    hasMultipleParticipants: {
      type: Boolean,
      default: false,
    },
    hasWeight: {
      type: Boolean,
      default: true,
    },
  },
  computed: {
    canSave () {
      if (!this.isNew && !this.hasChanged) {
        return false
      }
      return true
    },
  },
  methods: {
    maybeSave () {
      if (!this.canSave) return
      this.save()
    },
  },
}
</script>
