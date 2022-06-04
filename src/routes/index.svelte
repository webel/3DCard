<script lang="ts">
  import Card from "$components/Card.svelte";
</script>

<main>
  <Card />
  <Card
    cardConfig={{
      front: {
        background: "orange",
        title: "Banny",
        titleStyle: {
          size: {
            "font-size": "2em",
          },
          placement: { top: "25px" },
          simplePlacement: "center",
          color: "white",
        },
        image: "characters/01.png",
        imageStyle: {
          placement: { top: "50px", left: "-15px" },
          size: { height: "200px" },
        },
        logo: "characters/15.png",
      },
      back: {
        title: "Are you ready?",
        titleStyle: {
          color: "orange",
		  placement: {
			top: "55px",
			left: "15px",
		  },
          size: {
            "font-size": "1.5em",
          },
        },
        background: "black",
      },
      common: {},
    }}
  />
  <Card
    cardConfig={{
      front: {
        titleStyle: { placement: { top: "15px" }, simplePlacement: "center" },
        icons: [],
        image: "characters/21.png",
        imageStyle: {
          placement: { top: "35px", left: "-50px" },
          size: { width: "280px"}
        },
        logo: "characters/15.png",
      },
      back: {},
      common: {
		background: "slategray"
	  },
    }}
  />
</main>

<style>
  main {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
  }
</style>
