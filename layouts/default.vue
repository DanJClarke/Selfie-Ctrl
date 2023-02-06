<template>
    <div class>
        <nav>
            <ul>
                <li>
                    <NuxtLink to="/">Home</NuxtLink>
                </li>
                <li>
                    <NuxtLink to="selfies">Selfies</NuxtLink>
                </li>
                <li>
                    <NuxtLink to="about">About</NuxtLink>
                </li>
            </ul>
        </nav>
        Some shared layout content:

        <slot />
    </div>
</template>

<style scoped lang="scss">
.wrapper {
  margin: 0 auto;
  max-width: 390px;
  position: relative;
  width: 100%;
  height: 100vh;

}
.slide-card {
  height: calc(100vh - 7.5rem);
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1;
  background: #fff;
  border-radius: 0.625rem 0.625rem 0 0;
  display: flex;
  flex-direction: column;
  text-align: center;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: #444;
  line-height: 1.5;
  font-size: 14px;

  h1, h2 {
    font-size: 20px;
  }

  .percentage {
    font-size: 20px;
    font-weight: bold;
    color: #FE0B67;
  }
}
</style>