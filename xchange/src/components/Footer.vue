<template>
    <div class="footer-container">
        <footer>
            <p>Made with love by <a href="https://jdhasko.github.io">János Dominik Haskó</a></p>
        </footer>
    </div>
</template>

<script>
export default {
    name:'Footer'
}
</script>

<style scoped>
.footer-container
{
    position:fixed;
    bottom: 0%;
    width: 100%;
    background: black;
    color: white;
    padding: 5px 0;
}
p{
    margin: 0;
}
</style>