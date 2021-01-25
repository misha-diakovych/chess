<template>
    <div class="custom-select" :tabindex="tabindex" @blur="open = false">
        <div class="custom-select__selected" :class="{ open: open }" @click="open = !open">
            {{ selected }}
        </div>
        <div class="custom-select__items" :class="{ selectHide: !open }">
            <div
              v-for="(option, index) of options"
              :key="index"
              @click="selected = option;open = false;$emit('input', option);">
                {{ option }}
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'ActionSelect',
        props: {
            options: {
                type: Array,
                required: true,
            },
            default: {
                type: String,
                required: false,
                default: null,
            },
            tabindex: {
                type: Number,
                required: false,
                default: 0,
            },
        },
        data() {
            return {
                selected: this.default
                  ? this.default
                  : this.options.length > 0
                    ? this.options[0]
                    : null,
                open: false,
            };
        },
        mounted() {
            this.$emit("input", this.selected);
        },
    };
</script>

<style lang="scss" scoped>
    .custom-select {
        position: relative;
        width: 100%;
        text-align: left;
        outline: none;
        height: 40px;
        line-height: 40px;
        margin-top: 20px;
        &__selected {
            background-color: #0a0a0a;
            border-radius: 6px;
            border: 1px solid #666666;
            color: #fff;
            padding-left: 1em;
            cursor: pointer;
            user-select: none;
            &.open {
                border: 1px solid #ffffff;
                border-radius: 6px 6px 0 0;
            }
            &::after {
                position: absolute;
                content: "";
                top: 22px;
                right: 1em;
                width: 0;
                height: 0;
                border: 5px solid transparent;
                border-color: #fff transparent transparent transparent;
            }
        }
        &__items {
            color: #fff;
            border-radius: 0 0 6px 6px;
            overflow: hidden;
            border-right: 1px solid #fff;
            border-left: 1px solid #fff;
            border-bottom: 1px solid #fff;
            position: absolute;
            background-color: #0a0a0a;
            left: 0;
            right: 0;
            z-index: 1;
            div {
                color: #fff;
                padding-left: 1em;
                cursor: pointer;
                user-select: none;
            }
        }
    }
    .custom-select .items div:hover {
        background-color: #fff;
    }

    .selectHide {
        display: none;
    }
</style>

