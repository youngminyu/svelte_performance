<script>
  //   export let name;

  import { onMount, beforeUpdate, afterUpdate, onDestroy, tick } from "svelte";

  import TestInner from "./Inner.svelte";

  const colorClasses = ["primary", "secondary", "tertiary", "quaternary"];

  let LIST = [];
  let TIME = 0;
  let MEMORY_1 = 0;
  let MEMORY_2 = 0;

  let UPDATE_TIME = 0;
  let UPDATE = 0;

  let START;
  let END;
  let SUM = 0;

  let UPDATED = null;
  let CREATED = null;

  // afterUpdate(async (e) => {
  //   const c = document.getElementById("bowl").children;

  //   console.log("afterUpdate");

  //   if (CREATED === c.length) {
  //     END = performance.now();
  //     SUM = c.length;
  //     TIME = ((END - START) / 1000).toFixed(3);
  //     CREATED = null;
  //   }

  //   if (UPDATED !== null) {
  //     END = performance.now();
  //     UPDATE_TIME = ((END - START) / 1000).toFixed(3);
  //     UPDATED = null;
  //   }
  // });

  const changeFn = async (e) => {
    START = performance.now();
    const textValue = e.target.value;
    const length = textValue ? textValue.length : 0;
    const containerEl = document.querySelector(".container");
    const returnArr = [];

    const n = Math.min(Math.pow(length, 2), 1000);
    const height = containerEl.clientHeight / n;
    const width = containerEl.clientWidth / n;
    const counts = n * n;
    CREATED = counts;

    for (let i = 0; i < counts; i++) {
      const color =
        colorClasses[Math.floor(Math.random() * colorClasses.length)];
      //   returnArr.push(
      //     `style="height: ${height}px; width: ${width}px;" class="dot ${color}"`
      //   );
      returnArr.push({
        style: `height: ${height}px; width: ${width}px`,
        class: `dot ${color}`,
      });
    }
    LIST = returnArr;

    await tick();

    END = performance.now();
    SUM = document.getElementById("bowl").children.length;
    TIME = ((END - START) / 1000).toFixed(3);
    CREATED = null;
  };

  // setInterval(() => {
  //   const c = document.getElementById("bowl").children;
  //   if (CREATED === c.length) {
  //     END = performance.now();
  //     SUM = c.length;
  //     TIME = ((END - START) / 1000).toFixed(3);
  //     CREATED = null;
  //   }
  // });

  // setInterval(() => {
  //   const c = document.getElementById("bowl").children;
  //   const target = c[0];

  //   if (target) {
  //     const lastTarget = c[c.length - 1];
  //     const first = target.innerText;
  //     const end = lastTarget.innerText;
  //     if (UPDATED === +first && UPDATED === +end) {
  //       END = performance.now();
  //       UPDATE_TIME = ((END - START) / 1000).toFixed(3);
  //       UPDATED = null;
  //     }
  //   }
  // }, 1000);

  setInterval(() => {
    MEMORY_1 =
      "Memory :" +
      Math.round(window.performance.memory.usedJSHeapSize / 1000000) +
      "MB";
  }, 100);

  setInterval(() => {
    MEMORY_2 =
      "Total Memory :" +
      Math.round(window.performance.memory.totalJSHeapSize / 1000000) +
      "MB";
  }, 100);

  const myClick = () => {
    START = performance.now();
    UPDATED = true;
    UPDATE++;
  };
</script>

<style>
  :root {
    --progress-height: 40px;
    --progress-width: 500px;
    --progress-border-size: 15px;
    --progress-outer-height: calc(
      var(--progress-height) + var(--progress-border-size) * 2
    );
    --progress-outer-width: calc(
      var(--progress-width) + var(--progress-border-size) * 2
    );
  }

  html,
  body,
  .container {
    height: 100%;
    width: 100%;
    margin: 0;
    padding: 0;
  }
  #bowl {
    width: 100%;
    height: 100%;
    overflow: hidden;
    position: relative;
    z-index: 999;
  }

  button {
    background: #2196f3;
    color: #fff;
    font-size: 15px;
    display: block;
    border: none;
    width: 100%;
    margin-top: 20px;
    padding: 10px;
  }

  .container {
    overflow: hidden;
    z-index: 500;
    top: 0;
    left: 0;
    position: fixed;
    /* overflow: auto; */
    /* float: left; */
    height: 100%;
    width: 100%;
    margin: 0;
    padding: 0;
    /* position: relative; */
    overflow: hidden;
  }

  .dot {
    /* font-size: 0; */
    height: 25px;
    width: 25px;
    background-color: gray;
    float: left;
  }

  .memory {
    font-size: 30px;
    padding-top: 20px;
    font-weight: bold;
  }

  .dot.primary {
    background-color: #ff7c61;
  }

  .dot.secondary {
    background-color: #1affb0;
  }

  .dot.tertiary {
    background-color: #ffd336;
  }

  .dot.quaternary {
    background-color: #ffffff;
  }

  .my-input {
    position: fixed;
    width: 500px;
    z-index: 99999;
    padding-top: 70px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }

  .my-input input {
    font-size: 30px;
    height: 40px;
    width: 100%;
    box-sizing: border-box;
    top: 0;
    left: 0;
    position: absolute;
    z-index: 999;
  }

  .progress {
    z-index: 99;
    position: absolute;
    top: 0;
    left: 50%;
    transform: translate(-50%, -15px);
    height: calc(var(--progress-outer-height));
    width: calc(var(--progress-outer-width));
    background: linear-gradient(to right, #2196f3 99.99%, transparent),
      linear-gradient(to bottom, #2196f3 99.99%, transparent),
      linear-gradient(to right, #2196f3 99.99%, transparent),
      linear-gradient(to bottom, #2196f3 99.99%, transparent);
    background-size: 100% var(--progress-border-size),
      var(--progress-border-size) 100%, 100% var(--progress-border-size),
      var(--progress-border-size) 100%;
    background-repeat: no-repeat;
    animation: progress 3s linear infinite;
    background-position: calc(0px - var(--progress-outer-width)) 0px,
      calc(var(--progress-outer-width) - var(--progress-border-size))
        calc(0px - var(--progress-outer-height)),
      var(--progress-outer-width)
        calc(var(--progress-outer-height) - var(--progress-border-size)),
      0px var(--progress-outer-height);
  }

  @keyframes progress {
    0% {
      background-position: calc(0px - var(--progress-outer-width)) 0px,
        calc(var(--progress-outer-width) - var(--progress-border-size))
          calc(0px - var(--progress-outer-height)),
        var(--progress-outer-width)
          calc(var(--progress-outer-height) - var(--progress-border-size)),
        0px var(--progress-outer-height);
    }
    38% {
      background-position: 0px 0px,
        calc(var(--progress-outer-width) - var(--progress-border-size))
          calc(0px - var(--progress-outer-height)),
        var(--progress-outer-width)
          calc(var(--progress-outer-height) - var(--progress-border-size)),
        0px var(--progress-outer-height);
    }
    50% {
      background-position: 0px 0px,
        calc(var(--progress-outer-width) - var(--progress-border-size)) 0px,
        var(--progress-outer-width)
          calc(var(--progress-outer-height) - var(--progress-border-size)),
        0px var(--progress-outer-height);
    }
    88% {
      background-position: 0px 0px,
        calc(var(--progress-outer-width) - var(--progress-border-size)) 0px,
        0px calc(var(--progress-outer-height) - var(--progress-border-size)),
        0px var(--progress-outer-height);
    }
    100% {
      background-position: 0px 0px,
        calc(var(--progress-outer-width) - var(--progress-border-size)) 0px,
        0px calc(var(--progress-outer-height) - var(--progress-border-size)),
        0px 0px;
    }
  }
</style>

<div class="container" />
<div class="my-input">
  <div class="progress" />

  <input type="text" on:input={changeFn} />
  <div class="memory">{MEMORY_1}</div>
  <div class="memory">{MEMORY_2}</div>
  <div class="memory">Create Sec : {TIME} ({SUM})</div>
  <div class="memory">Update Sec : {UPDATE_TIME}</div>
  <button on:click={myClick}> 업데이트 </button>
</div>

<div id="bowl">
  {#each LIST as item}
    <div class={item.class} style={item.style}>
      <div>{UPDATE}</div>
      <!-- {UPDATE}
      <TestInner /> -->
      <!-- {#if UPDATE == 1}
        <div>{UPDATE}asdfasdf</div>
      {:else if UPDATE == 2}
        <span>{UPDATE}asdfasdf</span>
      {:else if UPDATE == 3}
        <p>{UPDATE}asdfasdf</p>
      {:else if UPDATE == 4}
        <a>{UPDATE}asdfasdf</a>
      {:else}<i>{UPDATE}asdfasdf</i>{/if} -->
    </div>
  {/each}
</div>
