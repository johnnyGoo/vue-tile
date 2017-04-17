<template>
    <div>
        <slot></slot>
    </div>
</template>


/*!
* vue-bar v0.0.1 (https://github.com/johnnyGoo/vue-bar)
* Author: Johnny chen
*
* Copyright 2013-2016 Johnny chen
*/
<script>
    var count = 0;
    if (!window.Smart) {
        throw 'vue-bar required smart.js'
    }
    var Smart = window.Smart;
    var Css = Smart.Css;
    var _ = Smart._;

    // 注册
    export default {
        // 声明 props
        props: {
            skip: {
                type: Boolean,
                default: false
            },
            state: {
                type: String,
                default: 'center'
            },
            transition: {
                type: String,
                default: 'all 0.4s cubic-bezier(.28,.14,.19,.99)'
            },
            center: {
                type: Object,
                default: function () {
                    return {x: 0, y: 0}
                }
            },
            left: {
                type: Object,
                default: function () {
                    return {x: '-100%', y: 0}
                }
            },
            right: {
                type: Object,
                default: function () {
                    return {x: '100%', y: 0}
                }
            },
            top: {
                type: Object,
                default: function () {
                    return {x: 0, y: '-100%'}
                }
            },
            bottom: {
                type: Object,
                default: function () {
                    return {x: 0, y: '100%'}
                }
            }, none: {
                type: Object,
                default: function () {
                    return {x: '-100%'}
                }
            }
        },
        data: function () {
            return {}
        },
        watch: {
            'state': function (n, o) {
                if (!_.contains([n, o], 'center') || _.contains([n, o], 'none')) {
                    this.update_transition(true)
                } else {
                    this.update_transition(this.skip)
                }
                this.update_state(n);
            },
            'skip': function (n, o) {
                this.update_transition(n)
            }
        }
        ,
        methods: {
            update_state: function (state) {
                var cssObj;
                switch (state) {
                    case "center":
                        cssObj = this.center;
                        break;
                    case "left":
                        cssObj = this.left;
                        break;
                    case "right":
                        cssObj = this.right;
                        break;
                    case "top":
                        cssObj = this.top;
                        break;
                    case "bottom":
                        cssObj = this.bottom;
                        break;
                    case "none":
                        cssObj = this.none;
                        break;
                }
                Css.smartCss(this.$el, cssObj)
            }
            , update_transition: function (skip) {
                if (skip) {
                    Css.smartCss(this.$el, {'transition': 'top 0s'});
                } else {
                    Css.smartCss(this.$el, {'transition': this.transition});
                }
            }
        },
        ready: function () {
            this.update_transition(this.skip);
            this.update_state(this.state);
        }
    }


</script>