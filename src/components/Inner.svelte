<script>
    import {createEventDispatcher} from "svelte";

    const dispatch = createEventDispatcher();

    const sayHello = () => {
        dispatch('message', {
            text: "hello"
        })
    }
</script>
<style>
.btn {
    background-color: aqua;
}
</style>
<button class="btn" on:click={sayHello} >
    CLick me
</button>