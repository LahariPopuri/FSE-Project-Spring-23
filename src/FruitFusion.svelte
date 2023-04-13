<script lang="js">
  import P5 from "p5-svelte";

  let passedInstructions = false;
  let screenImage;

  let totalScore = 0;

  let fruits = [
    {
      fruit: "apple",
      image: null,
      outLineImage: null,
      boxX: 500,
      boxY: 60,
      boxW: 55,
      boxH: 45,
      x: 30,
      y: 50,
      w: 50,
      b: 60,
      outLineX: 510,
      outLineY: 55,
      outLineW: 35,
      outLineH: 45,
      success: false,
    },
    {
      fruit: "pineapple",
      image: null,
      outLineImage: null,
      boxX: 500,
      boxY: 120,
      boxW: 50,
      boxH: 50,
      x: 30,
      y: 120,
      w: 50,
      b: 70,
      outLineX: 510,
      outLineY: 115,
      outLineW: 30,
      outLineH: 50,
      success: false,
    },
    {
      fruit: "grape",
      image: null,
      outLineImage: null,
      boxX: 500,
      boxY: 190,
      boxW: 60,
      boxH: 45,
      x: 30,
      y: 190,
      w: 50,
      b: 60,
      outLineX: 510,
      outLineY: 185,
      outLineW: 40,
      outLineH: 45,
      success: false,
    },
    {
      fruit: "mango",
      image: null,
      outLineImage: null,
      boxX: 500,
      boxY: 250,
      boxW: 50,
      boxH: 50,
      x: 30,
      y: 250,
      w: 50,
      b: 60,
      outLineX: 510,
      outLineY: 245,
      outLineW: 30,
      outLineH: 50,
      success: false,
    },
  ];
  const sketch = (p5) => {
    p5.preload = () => {
      screenImage = p5.loadImage("screen1.jpg");

      fruits[0].image = p5.loadImage("apple.png");
      fruits[1].image = p5.loadImage("pineapple.png");
      fruits[2].image = p5.loadImage("grape.png");
      fruits[3].image = p5.loadImage("mango.png");

      fruits[0].outLineImage = p5.loadImage("appleOutline.png");
      fruits[1].outLineImage = p5.loadImage("pineappleOutline.png");
      fruits[2].outLineImage = p5.loadImage("grapeOutline.png");
      fruits[3].outLineImage = p5.loadImage("mangoOutline.png");
    };

    p5.setup = () => {
      p5.createCanvas(600, 400);
    };

    p5.draw = () => {
      p5.image(screenImage, 0, 0);

      fruits.forEach((fruit) => {
        if (
          fruit.x + fruit.w / 2 > fruit.boxX &&
          fruit.x < fruit.boxX + fruit.boxW &&
          fruit.y + fruit.w / 2 > fruit.boxY &&
          fruit.y < fruit.boxY + fruit.boxH
        ) {
          p5.fill("green");
          fruit.success = true;
        } else {
          p5.fill("white");
          fruit.success = false;
        }

        p5.rect(fruit.boxX, fruit.boxY, fruit.boxW, fruit.boxH);
        p5.image(fruit.outLineImage, fruit.outLineX, fruit.outLineY);
        fruit.outLineImage.resize(fruit.outLineW, fruit.outLineH);

        p5.image(fruit.image, fruit.x, fruit.y);
        fruit.image.resize(fruit.w, fruit.b);

        p5.rect(fruit.x + fruit.w / 2, fruit.y + fruit.h / 2, 5, 5);
      });

      let success = fruits.filter((fruit) => fruit.success);
      totalScore = success.length * 25;
    };

    p5.mouseDragged = () => {
      fruits.forEach((fruit) => {
        if (
          p5.mouseX > fruit.x &&
          p5.mouseX < fruit.x + fruit.w &&
          p5.mouseY > fruit.y &&
          p5.mouseY < fruit.y + fruit.b
        ) {
          fruit.x = p5.mouseX - fruit.w / 2;
          fruit.y = p5.mouseY - fruit.b / 2;
        }
      });
    };
  };
</script>

<main>
  <h1 class="font-bold font-mono uppercase">Fruit Fusion</h1>

  {#if !passedInstructions}
    <div
      class="border-4 border-gray-400 bg-black rounded-xl p-3 m-3 text-xl font-mono w-fit"
    >
      <h2 class="font-sans font-extrabold uppercase text-3xl">Instructions</h2>
      <p>Start the game by pressing the “PLAY” button.</p>
      <p>The game will display fruits and their outlines.</p>
      <p>Tap on the fruit and drag it to the matching outline</p>
      <p>When the fruit is correctly matched, there will be a soft bell.</p>
    </div>
    <button
      class="font-mono font-extrabold text-3xl"
      on:click={() => {
        passedInstructions = true;
      }}>START</button
    >
  {:else}
    {#if totalScore >= 100}
      <h1 class="font-bold font-mono uppercase text-green-500">You Win!</h1>
      <h1>
        Score: {totalScore}
      </h1>
    {:else}
      <h1>
        Score: {totalScore}
      </h1>
    {/if}
    <div class="border-4 border-gray-400 p-2px rounded-xl">
      <P5 {sketch} />
    </div>
  {/if}
</main>
