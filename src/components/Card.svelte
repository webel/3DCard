<script lang="ts">
  import Icon from "./Icon.svelte";

  type CardFace = {
    background?: string;
    icons?: [
      {
        icon: string;
        iconPlacement: {
          top: string;
          right: string;
        };
      }
    ];
    title?: string;
    titlePlacement?: "top" | "bottom";
    titleSize?: "small" | "medium" | "large";
    titleColor?: string;
  };

  export let cardConfig: { front: CardFace; back: CardFace; common: CardFace } =
    {
      front: {
        title: "Ivan the Boneless",
      },
      back: {
        title: "The Banny Verse",
      },
      common: {
        icons: [
          {
            icon: "wifi",
            iconPlacement: {
              top: "12px",
              right: "12px",
            },
          },
        ],
      },
    };

  type CardConfig = typeof cardConfig;
</script>

<div class="scene">
  <div class="box">
    <div class="box__face box__face--front">
      <section>
        <header>
          <h1>{cardConfig.front.title}</h1>
          <Icon name="wifi" />
        </header>
        <div class="banny__container">
          <img id="banny" src="/characters/15.png" alt="Banny" />
        </div>
      </section>
    </div>
    <div class="box__face box__face--back">
      <section>
        <header>
          <Icon name="wifi" />
        </header>
        <h1>{cardConfig.back.title}</h1>
        <img class="logo" src="logo.png" alt="BannyVerse Logo" />
      </section>
    </div>
    <div class="box__face box__face--right" />
    <div class="box__face box__face--left" />
    <div class="box__face box__face--top" />
    <div class="box__face box__face--bottom" />
  </div>
</div>

<style>
  :root {
    --height: 300px;
    --width: 200px;
    --depth: 2px;

    --gradient-l0: linear-gradient(
      0deg,
      rgba(252, 252, 254, 1) 6%,
      rgba(236, 236, 236, 0.8477984943977591) 100%
    );
  }

  /************
  Card content styles
  */
  section {
    border: 1px solid #ccc;
    height: calc(var(--height) - 20px);
    width: calc(var(--width) - 20px);
    margin: 10px;
    border-radius: 5%;
  }

  section h1 {
    font-size: 12px;
    text-align: center;
    line-height: 0;
    font-family: "GalacticaGrid";
  }

  header {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 12px;
  }

  .logo {
    height: 60px;
    position: absolute;
    right: 0px;
    bottom: 1px;
  }

  .banny__container {
    width: 300px;
    position: relative;
  }

  #banny {
    width: 300px;
    position: absolute;
    left: -60px;
    top: -14px;
  }

  :global(#icon-wifi) {
    position: fixed;
    top: 14px;
    right: 16px;
  }

  .box__face--back h1 {
    font-size: 48px;
    line-height: 1;
  }
  /* ******* */

  .scene {
    width: var(--width);
    height: var(--height);
    margin: 80px;
    perspective: 400px;
  }

  .box {
    width: var(--width);
    height: var(--height);
    position: relative;
    transform-style: preserve-3d;
    transform: translateZ(-50px);
    transition: transform 1s;
    /* Animation as a combination of show-front and show-back */
    animation: show-front-and-back 10s ease-in-out infinite;
  }

  /* show-front-and-back animation */
  @keyframes show-front-and-back {
    0% {
      transform: rotateY(0deg);
    }
    50% {
      transform: rotateY(180deg);
    }

    100% {
      transform: rotateY(360deg);
    }
  }

  .box__face {
    position: absolute;
  }

  .box__face--front,
  .box__face--back {
    border-radius: 5%;
    width: var(--width);
    height: var(--height);
  }

  .box__face--right,
  .box__face--left {
    width: var(--depth);
    height: calc(var(--height) - 20px);

    top: 10px;
    left: 100px;
  }

  .box__face--top,
  .box__face--bottom {
    width: calc(var(--width) - 20px);
    height: var(--depth);

    top: 50px;
    left: 10px;
  }

  .box__face--front {
    background: linear-gradient(
      135deg,
      rgba(236, 236, 236, 1) 0%,
      rgba(236, 236, 236, 1) 40%,
      #ffffff 50%,
      rgba(236, 236, 236, 1) 60%,
      rgba(236, 236, 236, 1) 100%
    );
    background-position: center;
    background-size: 500%;
    animation: shimmer 10s linear infinite;
  }

  .box__face--back {
    background: linear-gradient(
      135deg,
      rgba(236, 236, 236, 1) 0%,
      rgba(236, 236, 236, 1) 40%,
      #ffffff 50%,
      rgba(236, 236, 236, 1) 60%,
      rgba(236, 236, 236, 1) 100%
    );
    background-position: center;
    background-size: 500%;
    animation: shimmer 10s linear infinite;
  }

  .box__face--top {
    background: linear-gradient(
      0deg,
      rgba(252, 252, 254, 1) 6%,
      rgba(236, 236, 236, 0.85) 100%
    );
  }
  .box__face--bottom {
    background: rgba(252, 252, 254, 1);
    background: linear-gradient(
      0deg,
      rgba(252, 252, 254, 1) 6%,
      rgba(236, 236, 236, 1) 100%
    );
  }

  /* Shimmer animation keyframes */
  @keyframes shimmer {
    0% {
      background-position: -100% 0;
    }
    100% {
      background-position: 100% 0;
    }
  }

  .box__face--front {
    transform: rotateY(0deg) translateZ(calc(var(--depth) / 2));
  }
  .box__face--back {
    transform: rotateY(180deg) translateZ(calc(var(--depth) / 2));
  }

  .box__face--right {
    transform: rotateY(90deg)
      translateZ(calc(var(--width) / 2 - var(--depth) / 2));
  }
  .box__face--left {
    transform: rotateY(90deg)
      translateZ(calc(var(--width) / 2 - var(--depth) / 2));
  }

  .box__face--top {
    transform: rotateX(90deg) translateZ(calc(50px + var(--depth) / 2));
  }
  .box__face--bottom {
    transform: rotateX(-90deg) translateZ(calc(250px - var(--depth) / 2));
  }

  /* Leaving these here for potential "manual" flips of the card */
  /* .box.show-front {
      transform: translateZ(-50px) rotateY(0deg);
    }
    .box.show-back {
      transform: translateZ(-50px) rotateY(-180deg);
    }
    .box.show-right {
      transform: translateZ(-150px) rotateY(-90deg);
    }
    .box.show-left {
      transform: translateZ(-150px) rotateY(90deg);
    }
    .box.show-top {
      transform: translateZ(-100px) rotateX(-90deg);
    }
    .box.show-bottom {
      transform: translateZ(-100px) rotateX(90deg);
    } */
</style>
