<script>
    export let cardId;

    function getCardImageUrl(id) {
        return `https://ygopro.online/assets/card-images/common/${id}.jpg`;
    }
</script>

<div
    class="card w-[50px] h-[72px] bg-cover bg-center rounded transition-transform duration-300 ease-out"
    style="background-image: url('{getCardImageUrl(cardId)}');"
></div>

<style>
    .card {
        transform-origin: center center;
        transition:
            transform 0.4s ease,
            box-shadow 0.4s ease;
    }

    .card:hover {
        transform-origin: center center;
        z-index: 20;
        box-shadow: 0 20px 40px rgba(0, 0, 0, 0.6);
        transform: scale(1.4) rotate3d(1, 2, -1, 18deg);
    }
</style>
