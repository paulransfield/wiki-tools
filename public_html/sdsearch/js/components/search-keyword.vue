<template>
    <div class="search-keyword"
         v-bind:expanded="expanded">
        <button class="search-keyword__button">
            <span class="icon"
                  v-bind:data-icon="keyword.icon"></span>
        </button>

        <div class="search-keyword__value"
             v-if="keyword.type === 'text'">
            <input type="text"
                   class="search-keyword__input"
                   v-on:input="input($event)"
                   v-bind:value="keyword.value" />
        </div>

        <wbstatement-entry
            v-if="keyword.type === 'wbstatement'"
            v-on:expand="expanded = true"
            v-on:contract="expanded = false"
            v-on:input="input($event)"
            v-bind:value="keyword.value"></wbstatement-entry>

        <button class="search-keyword__button"
                v-on:click="remove">
            <span class="icon"
                  data-icon="remove"></span>
        </button>
    </div>
</template>

<script>
    import WbstatementEntry from './wbstatement-entry.vue';

    function parseKeyword(keyword) {
        if (keyword.startsWith('haswbstatement')) {
            const icon = keyword.startsWith('haswbstatement:P180') ? 'image' : 'tag';

            return {
                icon : icon,
                type : 'wbstatement',
                value : keyword
            };
        } else {
            return {
                icon : 'text',
                type : 'text',
                value : keyword
            };
        }
    }

    export default {
        components : { WbstatementEntry },

        computed : {
            keyword() {
                return parseKeyword(this.value);
            }
        },

        data() {
            return {
                expanded : false
            };
        },

        methods : {
            input(e) {
                if (this.keyword.type === 'text') {
                    this.$emit('input', e.target.value);
                }

                if (this.keyword.type === 'wbstatement') {
                    this.$emit('input', e);
                }
            },

            remove() {
                this.$emit('remove');
            }
        },

        props : {
            value : {
                type : String
            }
        }
    }
</script>