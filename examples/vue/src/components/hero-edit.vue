<template>
<div class="hero-edit box">
    <h4>Edit</h4>
    <div class="alert" v-if="!hero.synced">
        <h4>Warning:</h4>
        <p>Someone else has <b>changed</b> this document. If you click save, you will overwrite the changes.</p>
        <button v-on:click="resync()">resync</button>
    </div>
    <div class="alert deleted" v-if="hero.deleted">
        <h4>Error:</h4>
        <p>Someone else has <b>deleted</b> this document. You can not save anymore.</p>
    </div>
    <h5>
      <div class="color-box" v-bind:style="{ backgroundColor: hero.color }"></div>
      {{hero.name}}
    </h5> HP: <input type="number" v-model.number="hero.hp" min="0" v-bind:max="hero.maxHP" name="hp" />
    <br />
    <button v-on:click="cancel()">cancel</button>
    <button v-on:click="submit()" v-if="!hero.deleted">submit</button>
</div>
</template>

<script>
import Vue from 'vue';
import * as Database from '../database/Database';
export default Vue.component('hero-edit', {
    data: () => {
        return {
            unsync: null,
            deleted: false,
            subs: []
        };
    },
    props: ['hero'],
    mounted: async function() {},
    beforeDestroy: function() {
        this.subs.forEach(sub => sub.unsubscribe());
    },
    methods: {
        async submit() {
            console.log('heroEdit.submit()');
            await this.hero.save();
            this.$emit('submit');
        },
        resync() {
            console.log('heroEdit.resync()');
            this.hero.resync();
        },
        cancel() {
            console.log('heroEdit.cancel()');
            this.hero.resync();
            this.$emit('cancel');
        }
    }
});
</script>


<style>
.hero-edit {
    position: fixed;
    z-index: 10;
    width: 70%;
    margin-left: 10%;
    min-height: 200px;
    margin-top: -5px;
    padding: 20px;
}

.alert {
    border-style: solid;
    border-width: 2px;
    border-radius: 10px;
    padding: 8px;
    border-color: #e0e021;
    &.deleted {
        border-color: red;
    }
    h4 {
        padding: 0;
        margin: 0;
    }
}

.color-box {
    width: 20px;
    height: 20px;
    float: left;
    margin-right: 11px;
    border-radius: 2px;
    border-width: 1px;
    border-style: solid;
    border-color: grey;
}
</style>
