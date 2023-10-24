<script>
  import data from './assets/fullData.json'
  import svelteLogo from './assets/svelte.svg'
  import map from './assets/map.svg'

  const total = data.length;
  let selectedCounty = data[Math.floor(Math.random() * total)];
  
  let scaleByPopulation = false;

  let zoom = 7;

  let mouse = {
    x: 0,
    y: 0
  }

  function setHover(id){
    selectedCounty = id;
    console.log(id);
  }

  function setRandom(){
    let rand = Math.floor(Math.random() * total);
    selectedCounty = data[rand];
    //console.log(data[rand]);
  }

  function toggleScaleByPopulation(){
    scaleByPopulation = !scaleByPopulation;
  }

  function changeZoom(e){
    // console.log(e)

    // mouse.x = e.x;
    // mouse.y = e.y
      if(e.deltaY > 0){
        if(zoom < 10){
          zoom++;
        }
      }
      else if(e.deltaY < 0){
        if(zoom > 1){
          zoom--;
        }
      }

  }
  
</script>

<main>
  <div style="position:fixed; left:50vw; top: 2vh; z-index: 20; background-color: #333; padding: 2em; border-radius: 7%">
    <h3> {selectedCounty.name + ', ' + selectedCounty.state} </h3>
    <h1> {selectedCounty.population.toLocaleString()} </h1>
    <!-- <h1> {zoom}</h1> -->
    <button on:click={setRandom}>
      Random
    </button>
    <button on:click={toggleScaleByPopulation}>
      Scale by Pop
    </button>
  </div>

  <div on:wheel={(e) => changeZoom(e)}>
    <svg width={1000} height={650} style="transform-origin: 0% 0%" transform="scale({7/zoom})">
      {#each data as county, i}
        <path 
          id={county.name + ", " + county.abbrv} 
          d={county.path} 
          on:click={() => setHover(county)}
          fill={county == selectedCounty ? "#47F": "#AAB"}
          stroke="#FFF"
          stroke-width={zoom/7}
          transform={scaleByPopulation ? "scale("+county.population*.0001+")" : null}
          >
    
          </path>
      {/each}
    </svg>
  </div>


</main>

<style>

</style>
