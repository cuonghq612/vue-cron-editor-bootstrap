<template>
    <b-tabs v-model="activeTab" @input="reset" content-class="mt-3">
        <b-tab :value="0" :title="_$t('minutes')" class="minutes-tab">
            <b-form inline>
                <label class="mr-sm-2">{{ _$t("every") }}</label>
                <b-form-input
                    type="number"
                    v-model="editorData.minuteInterval"
                    v-bind:style="{ width: '80px' }"
                />
                <label class="mr-sm-2 ml-sm-2">{{ _$t("mminutes") }}</label>
            </b-form>
        </b-tab>
        <b-tab :value="1" :title="_$t('hourly')" class="hourly-tab">
            <b-row>
                <b-col size="12">
                    <b-form inline>
                        <label class="mr-sm-2">{{ _$t("every") }}</label>
                        <b-form-input
                            type="number"
                            v-model="editorData.hourInterval"
                            v-bind:style="{ width: '80px' }"
                        />
                        <label class="mr-sm-2 ml-sm-2">{{
                            _$t("hoursOnMinute")
                        }}</label>
                        <b-form-input
                            type="number"
                            max="59"
                            min="0"
                            v-model="editorData.minutes"
                            v-bind:style="{ width: '80px' }"
                        />
                    </b-form>
                </b-col>
            </b-row>
        </b-tab>
        <b-tab :value="2" :title="_$t('daily')" class="daily-tab">
            <b-row>
                <b-col size="12">
                    <b-form inline>
                        <label class="mr-sm-2">{{ _$t("every") }}</label>
                        <b-form-input
                            type="number"
                            v-model="editorData.dayInterval"
                            v-bind:style="{ width: '80px' }"
                        />
                        <label class="mr-sm-2 ml-sm-2">{{
                            _$t("daysAt")
                        }}</label>
                        <b-form-timepicker
                            :value="dateTime"
                            :hour12="false"
                            @input="setDateTime"
                        />
                    </b-form>
                </b-col>
            </b-row>
        </b-tab>
        <b-tab :value="3" :title="_$t('weekly')" class="weekly-tab">
            <b-row>
                <b-col size="12">
                    <b-form inline>
                        <label class="mr-sm-2">{{ _$t("every") }}</label>
                        <b-form-checkbox
                            value="0"
                            class="mr-sm-2"
                            v-model="editorData.days"
                            switch
                            >{{ _$t("sun") }}</b-form-checkbox
                        >
                        <b-form-checkbox
                            value="1"
                            class="mr-sm-2"
                            v-model="editorData.days"
                            switch
                            >{{ _$t("mon") }}</b-form-checkbox
                        >
                        <b-form-checkbox
                            value="2"
                            class="mr-sm-2"
                            v-model="editorData.days"
                            switch
                            >{{ _$t("tue") }}</b-form-checkbox
                        >
                        <b-form-checkbox
                            value="3"
                            class="mr-sm-2"
                            v-model="editorData.days"
                            switch
                            >{{ _$t("wed") }}</b-form-checkbox
                        >
                        <b-form-checkbox
                            value="4"
                            class="mr-sm-2"
                            v-model="editorData.days"
                            switch
                            >{{ _$t("thu") }}</b-form-checkbox
                        >
                        <b-form-checkbox
                            value="5"
                            class="mr-sm-2"
                            v-model="editorData.days"
                            switch
                            >{{ _$t("fri") }}</b-form-checkbox
                        >
                        <b-form-checkbox
                            value="6"
                            v-model="editorData.days"
                            switch
                            >{{ _$t("sat") }}</b-form-checkbox
                        >

                        <label class="mr-sm-2 ml-sm-2">{{ _$t("at") }}</label>
                        <b-form-timepicker
                            :value="dateTime"
                            @input="setDateTime"
                            :hour12="false"
                        />
                    </b-form>
                </b-col>
            </b-row>
        </b-tab>
        <b-tab :value="4" :title="_$t('monthly')" class="monthly-tab">
            <b-row>
                <b-col size="12">
                    <b-form inline>
                        <label class="mr-sm-2">{{ _$t("onThe") }}</label>
                        <b-form-input
                            type="number"
                            v-model="editorData.day"
                            v-bind:style="{ width: '80px' }"
                        />
                        <label class="mr-sm-2 ml-sm-2">{{
                            _$t("dayOfEvery")
                        }}</label>
                        <b-form-input
                            type="number"
                            v-model="editorData.monthInterval"
                            v-bind:style="{ width: '80px' }"
                        /><label class="mr-sm-2 ml-sm-2">{{
                            _$t("monthsAt")
                        }}</label>
                        <b-form-timepicker
                            :value="dateTime"
                            @input="setDateTime"
                            :hour12="false"
                        />
                    </b-form>
                </b-col>
            </b-row>
        </b-tab>
        <b-tab :value="4" :title="_$t('advanced')" class="advanced-tab">
            <b-row>
                <b-col size="12">
                    <b-form inline>
                        <label class="mr-sm-2">{{
                            _$t("cronExpression")
                        }}</label>
                        <b-form-input v-model="editorData.cronExpression" />
                        <label class="mr-sm-2 ml-sm-2">
                            {{ explanation }}
                        </label>
                    </b-form>
                </b-col>
            </b-row>
        </b-tab>
    </b-tabs>
</template>

<script>
import vueCronEditorMixin from "./core/vueCronEditorMixin";
import {
    BTabs,
    BTab,
    BForm,
    BFormInput,
    BFormTimepicker,
    BFormCheckbox,
    BCol,
    BRow,
    BContainer
} from "bootstrap-vue";

export default {
    name: "VueCronEditorBootstrap",
    mixins: [vueCronEditorMixin],
    components: {
        BFormInput,
        BForm,
        BTabs,
        BTab,
        BCol,
        BRow,
        BFormTimepicker,
        BFormCheckbox
    },
    data: () => ({
        activeTab: null,
        tabs: [
            { id: 0, key: "minutes" },
            { id: 1, key: "hourly" },
            { id: 2, key: "daily" },
            { id: 3, key: "weekly" },
            { id: 4, key: "monthly" },
            { id: 5, key: "advanced" }
        ]
    }),
    mounted() {
        this.activeTab = this.tabs.find(t => t.key === this.currentTab).id;
    },
    watch: {
        currentTab() {
            this.activeTab = this.tabs.find(t => t.key === this.currentTab).id;
        }
    },
    computed: {
        dateTime() {
            let dateTime = `${this.editorData.hours}:${this.editorData.minutes}:00`;
            return dateTime;
        }
    },
    methods: {
        reset(e) {
            const tabKey = this.tabs.find(t => t.id === e).key;
            this._resetToTab(tabKey);
        },
        setDateTime(e) {
            if (e == null) {
                return;
            }
            const splittedTime = e.split(":");

            this.editorData.hours = splittedTime[0];
            this.editorData.minutes = splittedTime[1];
        }
    }
};
</script>
