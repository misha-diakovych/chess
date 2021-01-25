<template>
    <div v-click-outside="onOutsideClick"
         class="select-field-wrapper"
         @click="showListItems"
         @focus="disabled ? '' : onFocus()"
         @blur="showList = false"
         tabindex="0">
        <div class="t-new-action-select" :class="{ 'active-fields': showList === true }">
            <label v-if="!selectedOptionLimited" class="label" :class="{ 'selected': showList }">{{ label ? label : 'Select option' }}</label>
            <span v-else class="selected-value">{{ selectedOptionLimited }}</span>
            <span
                class="clear-selected-result"
                v-if="showIconClearSelectedResult && selectedOptionLimited"
                @click="clearSelectedResult"
                >×</span>
            <span class="icon-rotate" :class="{'active': showList}"></span>
        </div>

        <div class="select-dropdown-list" v-if="showList">
            <ul>
                <template v-if="actionOptions && actionOptions.length">
                    <li class="action-option" v-for="actionOption in actionOptions"
                        @click="actionOptionChoosed(actionOption)">
                        {{ actionOption.title }}
                    </li>
                    <div class="line"></div>
                </template>
                <li class="options-list" v-for="item in options" @click="optionChoosed(item)">
                    {{ cutLongString(item[trackVal]) }}
                </li>
            </ul>
        </div>
        <template v-if="errors && errors.length">
            <small v-for="error in errors" class="text-danger">
                {{ error }}
            </small>
        </template>
    </div>
</template>

<script>
    // Click-outside directive
    import vClickOutside from 'v-click-outside';

    export default {
        name: "ActionSelect",
        props: {
            nameLengthLimitation: Number,
            errors: Array,
            options: Array,
            trackVal: String,
            trackBy: String,
            actionOptions: Array,
            label: String,
            disabled: {
                default: false,
                type: Boolean
            },
            value: {},
            showIconClearSelectedResult: String,
        },
        directives: {
            clickOutside: vClickOutside.directive
        },
        data () {
            return {
                showList: false,
                selectedOptionLimited: null,
                useFocus: true,
                disabledOpenShowList: false
            }
        },
        computed: {
            /**
             * Find selected value if exist
             */
            selectedOption () {
                let neededItem;
                if (this.options) {
                    if (neededItem = this.options.find(item => item[this.trackBy] == this.value)) {
                        this.showList = false;
                        neededItem = neededItem[this.trackVal];
                    }
                }
                if(neededItem ) {
                    this.selectedOptionLimited = this.cutLongString(neededItem, this.nameLengthLimitation);
                }
                return neededItem;
            },
        },

        watch: {
            selectedOption: function (val) {
                if (!val)
                    this.selectedOptionLimited = null;
            }
        },
        methods: {

            /**
             * The function indicates whether you clicked or typed the tab key
             **/
            onFocus () {
                let self = this;
                setTimeout( () => {
                    if (self.useFocus === true && this.disabledOpenShowList === false) {
                        self.showList = !self.showList;
                    }
                }, 200);
            },

            /**
             * show list dropdown
             **/
            showListItems () {
                if (this.disabled && this.disabledOpenShowList === true)
                    return;

                this.showList = !this.showList;
                this.useFocus = false;
            },

            /**
             * On option change -> send event with new value
             **/
            optionChoosed(item) {
                let emit_data = this.trackBy ? item[this.trackBy] : item;
                this.$emit('input', emit_data);
            },

            /**
             * On option change -> send event with new value
             **/
            actionOptionChoosed(actionOption) {
                this.$emit(actionOption.event, actionOption);
            },

            /**
             * close popup when clicked outside the popup
             **/
            onOutsideClick() {
                this.showList = false;
                this.useFocus = true
            },

            /**
             * cut string if the length is bigger then nameLengthLimitation property
             *
             * @param string
             * @param limit
             * @returns {string}
             */
            cutLongString(string, limit = this.nameLengthLimitation) {
                return (string.length > limit) ? string.slice(0, limit) + '...' : string;
            },

            /**
             * On option change -> send event with new value
             **/
            clearSelectedResult () {
                this.disabledOpenShowList = true;
                this.$emit('clear');
            },
        },
    }
</script>

<style lang="scss">
    .select-field-wrapper {
        outline: none;
        position: relative;
        /*height: 50px;*/
        width: 100%;
        border-radius: 10px;
        background: rgba(239, 243, 245, 1);
        .t-new-action-select {
            cursor: pointer;
            border-radius: 5px;
            padding: 14px 15px 14px 17px;
            font-size: 15px;
            line-height: 15px;
            transition: all 0.1s linear;
            font-weight: 200;
            height: 50px;
            background: white;
            &.disabled {
                background-color: #e9ecef;
            }
            &:target {
                border: 2px solid #6294A2;
                tab-index: 8;
                outline: 0;
            }
            &:focus {
                border: 2px solid #6294A2;
                tab-index: 8;
                outline: 0;
            }

            &.active-fields {
                border-radius: 5px 5px 0 0;
            }
            .selected-value {
                color: #414A50;
            }
            .clear-selected-result {
                position: absolute;
                right: 28px;
                font-size: 24px;
                font-weight: 500;
                line-height: 19px;
                color: #414951;
                opacity: 1;
                display: block;
                cursor: pointer;
                top: 19px;
                user-select: none;
                z-index: 1;
            }

            .icon-rotate {
                position: absolute;
                right: 10px;
                background: url("../../../seller/images/iconset/arrow.svg");
                background-size: 100% 100%;
                width: 13px;
                height: 8px;
                transition: all .3s;
                top: 23px;
            }
            .icon-rotate.active {
                transform: rotate(-180deg);
            }
            .label {
                margin: 0;
                color: #93A4B3;
                font-size: 16px;
                line-height: 19px;
            }
        }

        .select-dropdown-list {
            position: absolute;
            width: 100%;
            background-color: #ffffff;
            z-index: 15;
            max-height: 230px;
            overflow: auto;
            border-radius: 0 0 10px 10px;
            box-shadow: 0 5px 8px -2px #00000029;
            top: 49px;
            .line {
                width: 100%;
                height: 1px;
                background: #BABEBF;
                margin: 15px 0;
            }

            ul {
                margin: 0;
                padding: 0px 15px 15px 17px;
            }

            .action-option {
                line-height: 19px;
                font-size: 14px;
                color: #009993;
                font-weight: 600;
                cursor: pointer;
                text-decoration: underline;
                list-style-type: none;
            }

            .options-list {
                font-size: 16px;
                line-height: 19px;
                cursor: pointer;
                list-style-type: none;
                color: #3A4146;
                font-weight: 200;
                padding: 16px 0;
                &:hover {
                    background: #DDDDDD;
                    display: flex;
                    align-items: center;
                    margin: 0 -17px;
                    padding: 16px 17px;
                }
            }
        }
    }
</style>
