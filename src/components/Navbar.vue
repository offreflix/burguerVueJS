<template>
    <nav class="nav">
        <router-link to="/" class="logoUrl">
            <img :src="logo" :alt="alt" class="logo">
        </router-link>
        <router-link to="/">Home</router-link>
        <router-link to="/requests">Pedidos</router-link>
    </nav>
</template>

<script>
export default {
    name: "Navbar",
    props: ["logo", "alt"]
}
</script>

<style scoped>
.nav {
    background-color: #222;
    border-bottom: 4px solid #111;
    padding: 15px 20px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
}

.nav .logoUrl {
    margin: auto;
    margin-left: 0;
}

.logo {
    width: 40px;
    height: 40px;
}


.nav a {
    color: #FCBA03;
    text-decoration: none;
    margin: 6px;
    transition: .5s;
}

.nav a:hover {
    color: #FFF;
}
</style>