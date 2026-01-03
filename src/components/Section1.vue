<template>
  <section class="container section-1">
    <div class="row">
      <!-- COLUMN 2 -->
      <!--  -->
      <div class="column">
        <div class="wrapper-nav">
          <div>
            <p>plan</p>
            <div></div>
          </div>
          <div>
            <p>design</p>
            <div></div>
          </div>
          <div>
            <p>build</p>
            <div></div>
          </div>
        </div>

        <div class="container-list">
          <!-- item-1 -->
          <div class="list-item is-active">
            <img class="icon" src="@/assets/images/hamburger.svg" alt="" />
            <h2>The sitemap of the experience</h2>
            <p>
              Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
              nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam
              erat, sed diam voluptua. At vero eos et accusam et justo duo
              dolores et ea rebum. Stet clita kasd gubergren, no sea
            </p>
            <div class="mobile-img is-active">
              <img class="table" src="@/assets/images/table_xl.jpg" alt="" />
            </div>
          </div>

          <!-- item-2 -->
          <div class="list-item">
            <img class="icon" src="@/assets/images/globe.svg" alt="" />
            <h2>Time to paint the room walls</h2>
            <p>
              Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
              nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam
              erat, sed diam voluptua. At vero eos et accusam et justo duo
              dolores et ea rebum.
            </p>
            <div class="mobile-img">
              <img class="table" src="@/assets/images/wall_xl.jpg" alt="" />
            </div>
          </div>

          <!-- item-3 -->
          <div class="list-item">
            <img class="icon" src="@/assets/images/tag.svg" alt="" />
            <h2>Magic happens to build it out</h2>
            <p>
              Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
              nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam
              erat, sed diam voluptua. At vero eos et accusam et.
            </p>
            <div class="mobile-img">
              <img class="table" src="@/assets/images/men_xl.jpg" alt="" />
            </div>
          </div>
        </div>
      </div>

      <!-- COLUMN 2 -->
      <!--  -->
      <div class="column">
        <div class="wrapper-image">
          <div class="list-item is-active">
            <img class="icon" src="@/assets/images/hamburger.svg" alt="" />
            <img
              class="table img-photo"
              src="@/assets/images/table_xl.jpg"
              alt=""
            />
          </div>

          <div class="list-item">
            <img class="icon" src="@/assets/images/globe.svg" alt="" />
            <img
              class="wall img-photo"
              src="@/assets/images/wall_xl.jpg"
              alt=""
            />
          </div>

          <div class="list-item">
            <img class="icon" src="@/assets/images/tag.svg" alt="" />
            <img
              class="men img-photo"
              src="@/assets/images/men_xl.jpg"
              alt=""
            />
          </div>
        </div>
        <p><span>1</span> / <span>3</span></p>
      </div>
    </div>
  </section>
</template>

<script>
import { gsap } from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      currentParagraph: {},
    };
  },
  mounted() {
    const section = document.querySelector(".section-1");
    const textItems = section.querySelectorAll(
      ".column:nth-child(1) .container-list .list-item"
    );
    const imageItems = section.querySelectorAll(
      ".column:nth-child(2) .wrapper-image .list-item"
    );

    const image_photo = section.querySelectorAll(
      ".column:nth-child(2) .wrapper-image .list-item .img-photo"
    );

    const icon = section.querySelectorAll(
      ".column:nth-child(2) .wrapper-image .list-item .icon"
    );

    const number = section.querySelector(".column:nth-child(2) p span");

    const total = textItems.length;
    ScrollTrigger.create({
      trigger: section,
      start: "top top",
      end: `+=${total * 70}%`,
      scrub: true,
      pin: true,
      onUpdate: (self) => {
        const index = Math.min(Math.floor(self.progress * total), total - 1);

        // STEP BASED
        textItems.forEach((el, i) =>
          el.classList.toggle("is-active", i === index)
        );

        imageItems.forEach((el, i) =>
          el.classList.toggle("is-active", i === index)
        );

        // SCROLL BASED SIZE
        const resizeImagesPhoto = (elements, minScale = 1, maxScale = 1.3) => {
          elements.forEach((el, i) => {
            const start = i / total;
            const end = (i + 1) / total;

            let p = (self.progress - start) / (end - start);

            const scale = maxScale - (maxScale - minScale) * p;

            el.style.transform = `scale(${scale})`;
          });
        };

        const resizeIcons = (elements, minScale = 1, maxScale = 1.3) => {
          elements.forEach((el, i) => {
            const start = i / total;
            const end = (i + 1) / total;

            let p = (self.progress - start) / (end - start);

            const scale = minScale + (maxScale - minScale) * p;

            el.style.transform = `translate(-50%, -50%) scale(${scale})`;
          });
        };

        // icon → LEBIH KECIL
        resizeImagesPhoto(image_photo, 1, 1.1);
        resizeIcons(icon, 1.2, 1.4);

        number.innerHTML = index + 1;
      },
    });
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/styles/variables";
@import "@/assets/styles/mixins";

.container.section-1 {
  background-color: #111606;
  height: 100vh;

  & .row {
    height: 100%;
    box-sizing: border-box;
    display: flex;

    & .column {
      flex: 1;
      position: relative;

      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: stretch;
      overflow: hidden;
      box-sizing: border-box;
    }

    & .column:nth-child(1) {
      padding: 3em;

      & .wrapper-nav {
        display: flex;
        gap: 0.88em;

        & p {
          font-size: 2em;
          line-height: 0.9;
          letter-spacing: -0.02em;
          cursor: pointer;
        }
      }

      & .container-list {
        // position: relative;

        & .list-item {
          display: none;

          &.is-active {
            display: block;
          }

          & .icon {
            display: none;
          }

          & h2 {
            max-width: 450px;
            margin-top: 0px;
            margin-bottom: 0px;
            padding-bottom: 20px;
            font-family: "Recife Display", serif;
            font-size: 4em;
            line-height: 0.8311688311688312;
            font-weight: 400;
            letter-spacing: -0.02em;
          }

          & p {
            max-width: 350px;
            font-size: 16px;
            overflow-wrap: break-word;
          }

          & .mobile-img {
            display: none;
          }
        }
      }
    }

    & .column:nth-child(2) {
      position: relative;
      border-radius: 7%;
      padding: 3em;

      & .wrapper-image {
        & .list-item {
          display: none;

          &.is-active {
            display: block;
          }

          & .table,
          .wall,
          .men {
            position: absolute;
            left: 0%;
            top: 0%;
            right: 0%;
            bottom: 0%;
            width: 100%;
            height: 100%;
            transform: scale(1.2);
            // transition: 1s;
            object-fit: cover;

            // &.is-active {
            //   transform: scale(1.4);
            // }
          }

          & .icon {
            position: absolute;
            width: 4em;
            z-index: 9;
            transform: translate(-50%, -50%);
            max-width: 70%;
            top: 50%;
            left: 50%;

            // &.is-active {
            //   transform: translate(-50%, -50%) scale(1);
            // }
          }
        }
      }
      & p {
        z-index: 2;
        font-style: italic;
        font-family: "Recife Display", sans-serif;
        font-size: 2.25em;
        position: absolute;
        top: 40px;
        right: 45px;
      }
    }
  }
}
</style>
