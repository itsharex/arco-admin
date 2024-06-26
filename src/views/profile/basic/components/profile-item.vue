<template>
  <div class="item-container">
    <a-space :size="16" direction="vertical" fill>
      <a-descriptions
        v-for="(item, idx) in blockDataList"
        :key="idx"
        :label-style="{
          textAlign: 'right',
          width: '200px',
          paddingRight: '10px',
          color: 'rgb(var(--gray-8))'
        }"
        :value-style="{ width: '400px' }"
        :title="item.title"
        :data="item.data"
      >
        <template #value="{ value }">
          <a-skeleton v-if="loading" :animation="true">
            <a-skeleton-line :widths="['200px']" :rows="1" />
          </a-skeleton>
          <span v-else>{{ value }}</span>
        </template>
      </a-descriptions>
    </a-space>
  </div>
</template>

<script lang="ts" setup>
import { computed, PropType } from 'vue';
import { useI18n } from 'vue-i18n';
import { ProfileBasicRes } from '@/api/profile';

type BlockList = {
  title: string;
  data: {
    label: string;
    value: string;
  }[];
}[];

const props = defineProps({
  type: {
    type: String,
    default: ''
  },
  renderData: {
    type: Object as PropType<ProfileBasicRes>,
    required: true
  },
  loading: {
    type: Boolean,
    default: false
  }
});
const { t } = useI18n();
const blockDataList = computed<BlockList>(() => {
  const { renderData } = props;
  const result = [];
  result.push({
    title:
      props.type === 'pre' ? t('basicProfile.title.preVideo') : t('现视频参数'),
    data: [
      {
        label: '匹配模式',
        value: renderData?.video?.mode || '-'
      },
      {
        label: '采集分辨率',
        value: renderData?.video?.acquisition.resolution || '-'
      },
      {
        label: '采集帧率',
        value: `${renderData?.video?.acquisition.frameRate || '-'} fps`
      },
      {
        label: '编码分辨率',
        value: renderData?.video?.encoding.resolution || '-'
      },
      {
        label: '编码码率最小值',
        value: `${renderData?.video?.encoding.rate.min || '-'} bps`
      },
      {
        label: '编码码率最大值',
        value: `${renderData?.video?.encoding.rate.max || '-'} bps`
      },
      {
        label: '编码码率默认值',
        value: `${renderData?.video?.encoding.rate.default || '-'} bps`
      },
      {
        label: '编码帧率',
        value: `${renderData?.video?.encoding.frameRate || '-'} fpx`
      },
      {
        label: '编码profile',
        value: renderData?.video?.encoding.profile || '-'
      }
    ]
  });

  result.push({
    title:
      props.type === 'pre'
        ? t('basicProfile.title.preAudio')
        : t('basicProfile.title.audio'),
    data: [
      {
        label: t('basicProfile.label.audio.mode'),
        value: renderData?.audio?.mode || '-'
      },
      {
        label: t('basicProfile.label.audio.acquisition.channels'),
        value: `${renderData?.audio?.acquisition.channels || '-'} ${t(
          'basicProfile.unit.audio.channels'
        )}`
      },
      {
        label: t('basicProfile.label.audio.encoding.channels'),
        value: `${renderData?.audio?.encoding.channels || '-'} ${t(
          'basicProfile.unit.audio.channels'
        )}`
      },
      {
        label: t('basicProfile.label.audio.encoding.rate'),
        value: `${renderData?.audio?.encoding.rate || '-'} kbps`
      },
      {
        label: t('basicProfile.label.audio.encoding.profile'),
        value: renderData?.audio?.encoding.profile || '-'
      }
    ]
  });

  return result;
});
</script>

<style scoped lang="less">
.item-container {
  padding-top: 20px;

  :deep(.arco-descriptions-item-label) {
    font-weight: normal;
  }
}
</style>
