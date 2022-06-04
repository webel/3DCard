<script lang="ts">
  import Icon from "./Icon.svelte";

  type Placement = {
    // TODO how do I force the type to be atleast top/bottom or left/right?
    top?: string;
    right?: string;
    bottom?: string;
    left?: string;
  };

  type Size = {
    width?: string;
    height?: string;
    "font-size"?: string;
  };

  type TextStyle = {
    color?: string;
    placement?: Placement;
    simplePlacement?: "center" | "left" | "right";
    size?: Size;
  };

  type ImageStyle = {
    placement?: Placement;
    size?: Size;
  };

  type CardFace = {
    background?: string;
    icons?:
      | [
          {
            icon: string;
            // NOTE: Icons are svg's and are thus styleable with textStyle
            iconStyle?: TextStyle;
          }
        ]
      | [];
    image?: string;
    imageStyle?: ImageStyle;
    logo?: string;
    logoStyle?: ImageStyle;
    title?: string;
    titleStyle?: TextStyle;
    leftVertical?: string;
    leftVerticalStyle?: TextStyle;
    rightVertical?: string;
    rightVerticalStyle?: TextStyle;
  };

  export let cardConfig: { front: CardFace; back: CardFace; common: CardFace } =
    {
      front: {
        title: "Ivan the Boneless",
        titleStyle: { placement: { top: "15px" }, simplePlacement: "center" },
        icons: [],
        image: "characters/15.png",
        imageStyle: {
          size: { height: "300px" },
          placement: { left: "-60px", top: "18px" },
        },
      },
      back: {
        title: "The Banny Verse",
        titleStyle: {
          size: { "font-size": "48px" },
        },
        icons: [],
        logo: "logo.png",
        logoStyle: {
          placement: {
            bottom: "-3px",
            right: "-15px",
          },
          size: { height: "100px" },
        },
      },
      common: {
        background: "rgba(236, 236, 236, 1)",
        icons: [
          {
            icon: "wifi",
            iconStyle: {
              placement: {
                top: "12px",
                right: "12px",
              },
            },
          },
        ],
      },
    };

  const { back, front, common } = cardConfig;

  const backIcons = [...(common.icons || []), ...(back.icons || [])];
  const frontIcons = [...(common.icons || []), ...(front.icons || [])];

  function getStyleString(style: ImageStyle | TextStyle | undefined) {
    if (!style) {
      return "";
    }
    const { color, placement, size } = style;
    const placementString = Object.entries(placement || {})
      .map(([key, value]) => `${key}: ${value}`)
      .join(";");
    const sizeString = Object.entries(size || {})
      .map(([key, value]) => `${key}: ${value}`)
      .join(";");

    const styleString = `${placementString}; ${sizeString}; color: ${color};`;

    if (placement) {
      return `${styleString}; position: absolute;`;
    }
    return `${styleString};`;
  }

  function getBackgroundString(background: string | undefined) {
    if (!background) {
      return "";
    }
    // TODO background options of animations;
    // - shimmer
    // - sparkles
    // - degrees of gradient
    return `background: linear-gradient(
      135deg,
      ${background} 0%,
      ${background} 40%,
      #ffffff 50%,
      ${background} 60%,
      ${background} 100%
    );
    background-size: 500%;
    background-position: center;
    `;
  }
</script>

<div class="scene">
  <div class="box">
    <div
      class="box__face box__face--front"
      style={getBackgroundString(front.background || common.background)}
    >
      <section>
        {#if front.title}
          <h1
            class:center={front.titleStyle?.simplePlacement === "center"}
            style={getStyleString(front.titleStyle)}
          >
            {front.title}
          </h1>
        {/if}
        {#each frontIcons as icon}
          <Icon name={icon.icon} />
        {/each}
        <div class="image__container">
          <img
            src={front.image}
            alt="Banny"
            style={getStyleString(front.imageStyle)}
          />
        </div>
      </section>
    </div>
    <div
      class="box__face box__face--back"
      style={getBackgroundString(back.background || common.background)}
    >
      <section>
        {#each backIcons as icon}
          <Icon name={icon.icon} />
        {/each}
        {#if back.title}
          <h1 style={getStyleString(back.titleStyle)}>{back.title}</h1>
        {/if}
        {#if back.logo || common.logo}
          <img
            src={back.logo || common.logo}
            alt="BannyVerse Logo"
            style={getStyleString(back.logoStyle)}
          />
        {/if}
        {#if back.image || common.image}
          <div class="image__container">
            <img
              src={back.image || common.image}
              alt="Banny"
              style={getStyleString(front.imageStyle)}
            />
          </div>
        {/if}
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

  .center {
    text-align: center;
    width: calc(var(--width) - 20px);
  }

  .image__container {
    width: 300px;
    position: relative;
  }

  :global(#icon-wifi) {
    position: fixed;
    top: 14px;
    right: 16px;
  }

  .box__face--back h1 {
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

  .box__face--front,
  .box__face--back {
    animation: shimmer 10s linear infinite;
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
