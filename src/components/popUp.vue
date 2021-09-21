<template>
    <div class="popUp" v-bind:class="{visible: this.error}">
        <div class="overlay">
            <article class="error-wrapper">
                <button @click="$emit('remove-popup')" class="close-btn">&times;</button>
                <div class="error-title">
                    <h2>Sorry, something went wrong.</h2>
                    <p>Please retry later.</p>
                </div>
                <div class="error-description">
                    <p>{{error}}</p>
                </div>
            </article>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'App',
        props: {
            error: {
                type: Error || null,
            }
        },
    }
</script>

<style>
    .popUp {
        display: none;
    }

    .overlay {
        position: absolute;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        z-index: 10;
        background-color: rgba(0,0,0,0.2);
    }

    .visible {
        display: block;
    }

    .error-wrapper {
        display: flex;
        flex-direction: column;
        justify-content: center;
        position: relative;
        left: 50%;
        top: 20%;
        transform: translateX(-50%);
        z-index: 11;
        width: 40vw;
        height: 300px;
        background-color: white;
    }

    .close-btn{
        position: absolute;
        top: 0;
        right: 0;
        width: 20px;
        height: 20px;
        display: flex;
        justify-content: center;
        align-items: center;
        background: red;
        color: #fff;
        border-radius: 50%;
        font-weight: bold;
        cursor: pointer;
        text-align: center;
    }
</style>