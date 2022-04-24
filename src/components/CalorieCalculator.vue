<script setup>
import { ManSharp, WomanSharp } from '@vicons/ionicons5';
import { ref, computed, watch } from 'vue';

const gender = ref('male');
const age = ref(25);
const weight = ref(0);
const height = ref(0);
const activityMultiplier = ref(1);
const goalAdjust = ref(0);

const activityLevelList = [
  {
    title: 'Sedentary',
    multiplier: 1.1,
  },
  {
    title: 'Lightly Active',
    multiplier: 1.275,
  },
  {
    title: 'Moderately Active',
    multiplier: 1.45,
  },
  {
    title: 'Very Active',
    multiplier: 1.625,
  },
  {
    title: 'Extra Active',
    multiplier: 1.8,
  },
];
const goalList = [
  {
    title: 'Lose Weight',
    adjust: -250,
  },
  {
    title: 'Gain Weight',
    adjust: 250,
  },
  {
    title: 'Maintain Weight',
    adjust: 0,
  },
];

const bmr = computed(() => {
  const baseval = 10 * weight.value + 6.25 * height.value - 5 * age.value;
  if (gender.value === 'male') {
    return baseval + 5;
  }
  return baseval - 161;
});

const maintenance = computed(() => bmr.value * activityMultiplier.value);
const targetCalories = computed(() => maintenance.value + goalAdjust.value);
const targetProtein = computed(() => weight.value * 1.6);

const bmrAnimation = ref(null);
const maintAnimation = ref(null);
const targetCalAnimation = ref(null);
const proteinAnimation = ref(null);

const bmrOld = ref(0);
const bmrNew = ref(0);

const maint = ref({ old: 0, new: 0 });
const targetcal = ref({ old: 0, new: 0 });
const targetprot = ref({ old: 0, new: 0 });

watch(bmr, (newval, oldval) => {
  bmrOld.value = oldval;
  bmrNew.value = newval;
  bmrAnimation.value?.play();
});
watch(maintenance, (newval, oldval) => {
  maint.value.old = oldval;
  maint.value.new = newval;
  maintAnimation.value?.play();
});
watch(targetCalories, (newval, oldval) => {
  targetcal.value.old = oldval;
  targetcal.value.new = newval;
  targetCalAnimation.value?.play();
});
watch(targetProtein, (newval, oldval) => {
  targetprot.value.old = oldval;
  targetprot.value.new = newval;
  proteinAnimation.value?.play();
});
</script>

<template>
  <n-space justify="space-around" size="large">
    <n-card class="calculator calccard" title="Calorie Calculator">
      <n-space vertical>
        <n-form
          ref="caloriecalc"
          :model="calc"
          label-placement="left"
          :label-width="160"
        >
          <n-form-item label="Calculate values for">
            <n-radio-group v-model:value="gender" name="gender">
              <n-radio-button value="male">
                <n-icon :component="ManSharp" size="25" />
              </n-radio-button>
              <n-radio-button value="female">
                <n-icon :component="WomanSharp" size="25" />
              </n-radio-button>
            </n-radio-group>
          </n-form-item>
          <n-form-item label="Age">
            <n-input-number v-model:value="age" />
          </n-form-item>
          <n-form-item label="Weight (kg)">
            <n-input-number v-model:value="weight" />
          </n-form-item>
          <n-form-item label="Height (cm)">
            <n-input-number v-model:value="height" />
          </n-form-item>
          <n-form-item label="Activity Level">
            <n-radio-group
              v-model:value="activityMultiplier"
              name="activityMultiplier"
            >
              <n-radio-button
                v-for="al in activityLevelList"
                :key="al.multiplier"
                :value="al.multiplier"
                :label="al.title"
              />
            </n-radio-group>
          </n-form-item>
          <n-form-item label="Goal">
            <n-radio-group v-model:value="goalAdjust" name="goalAdjust">
              <n-radio-button
                v-for="goal in goalList"
                :key="goal.title"
                :value="goal.adjust"
                :label="goal.title"
              />
            </n-radio-group>
          </n-form-item>
        </n-form>
      </n-space>
    </n-card>
    <n-card title="Results" class="calculated calccard">
      <n-descriptions label-placement="left" :column="1">
        <n-descriptions-item label="BMR (Calories)">
          <n-number-animation
            ref="bmrAnimation"
            :from="bmrOld"
            :to="bmrNew"
            :active="true"
            :precision="0"
          />
        </n-descriptions-item>
        <n-descriptions-item label="Maintenance">
          <n-number-animation
            ref="maintAnimation"
            :from="maint.old"
            :to="maint.new"
            :active="true"
            :precision="0"
          />
        </n-descriptions-item>
        <n-descriptions-item label="Target Calories">
          <n-number-animation
            ref="targetCalAnimation"
            :from="targetcal.old"
            :to="targetcal.new"
            :active="true"
            :precision="0"
          />
        </n-descriptions-item>
        <n-descriptions-item label="Target Protein (grams)">
          <n-number-animation
            ref="proteinAnimation"
            :from="targetprot.old"
            :to="targetprot.new"
            :active="true"
            :precision="0"
          />
        </n-descriptions-item>
      </n-descriptions>
    </n-card>
  </n-space>
</template>

<style lang="scss" scoped>
.calccard {
  min-width: 40vw;
  margin-top: 30px;
}
</style>
