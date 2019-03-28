<template>
    <div 
         v-show="show"
         v-click-outside="close"
         :style="style"
         class="v-context dropdown-menu"
         tabindex="-1"
         @click="onClick"
         @contextmenu.capture.prevent
    >
        <slot :data="data" />
    </div>
</template>

<script>
    import ClickOutside from 'vue-click-outside';

    export default {
        name: 'VueContext',

        directives: {
            ClickOutside,
        },

        props: {
            /**
             * Close the menu on click.
             *
             * @type {boolean}
             */
            closeOnClick: {
                type: Boolean,
                default: true
            },

            /**
             * Close the menu automatically on window scroll.
             *
             * @type {boolean}
             */
            closeOnScroll: {
                type: Boolean,
                default: true
            }
        },

        computed: {
            /**
             * Generate the CSS styles for positioning the context menu.
             *
             * @returns {object|null}
             */
            style () {
                return this.show
                    ? { top: `${this.top}px`, left: `${this.left}px` }
                    : null;
            }
        },

        data () {
            return {
                top: null,
                left: null,
                show: false,
                data: null
            };
        },

        mounted () {
            if (this.closeOnScroll) {
                this.addScrollEventListener();
            }
        },

        beforeDestroy () {
            if (this.closeOnScroll) {
                this.removeScrollEventListener();
            }
        },

        methods: {
            /**
             * Add scroll event listener to close context menu.
             */
            addScrollEventListener () {
                window.addEventListener('scroll', this.close);
            },

            /**
             * Close the context menu.
             *
             * @param {boolean|Event} emit Used to prevent event being emitted twice from when menu is clicked and closed
             */
            close (emit = true) {
                if (! this.show) {
                    return;
                }

                this.top = null;
                this.left = null;
                this.data = null;
                this.show = false;

                if (emit) {
                    this.$emit('close');
                }
            },

            /**
             * Close the menu if `closeOnClick` is set to true.
             */
            onClick () {
                if (this.closeOnClick) {
                    this.close(false);
                }
            },

            /**
             * Open the context menu.
             *
             * @param {MouseEvent} event
             * @param {array|object|string} data User provided data for the menu
             */
            open (event, data) {
                this.data = data;
                this.show = true;

                this.$nextTick(() => {
                    this.positionMenu(event.clientY, event.clientX);
                    this.$el.focus();

                    this.$emit('open', event, this.data, this.top, this.left);
                });
            },

            /**
             * Set the context menu top and left positions.
             *
             * @param {number} top
             * @param {number} left
             */
            positionMenu (top, left) {
                const largestHeight = window.innerHeight - this.$el.offsetHeight - 25;
                const largestWidth = window.innerWidth - this.$el.offsetWidth - 25;

                if (top > largestHeight) {
                    top = largestHeight;
                }

                if (left > largestWidth) {
                    left = largestWidth;
                }

                this.top = top;
                this.left = left;
            },

            /**
             * Remove the scroll event listener to close the context menu.
             */
            removeScrollEventListener () {
                window.removeEventListener('scroll', this.close);
            }
        },

        watch: {
            /**
             * Add or remove the scroll event listener when the prop value changes.
             *
             * @param {boolean} value
             * @param {boolean} oldValue
             */
            closeOnScroll (value, oldValue) {
                if (value === oldValue) {
                    return;
                }

                if (value) {
                    this.addScrollEventListener();
                } else {
                    this.removeScrollEventListener();
                }
            }
        }
    };
</script>

<style lang="scss" scoped>
    .v-context {
        display: block;
        margin: 0;
        position: fixed;
        width: 250px;
        z-index: 99999;
    }
</style>
