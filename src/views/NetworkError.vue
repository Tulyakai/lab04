<template>
    <div class="networkError">
        <h1>Uh-Oh!</h1>
        <h3>It looks loke you're experiencing some network issues, please take a breath and
        <a href="#" @click="$router.go(-1)">Click here</a> to try again.
        </h3>
        <router-link :to="{name: 'EventList'}">Back to home page</router-link>
    </div>
</template>