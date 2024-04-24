
<script>
  import * as d3 from "d3"
  import { onMount } from "svelte"

  let usuarios = []
  
  let colorIntensidad = d3
    .scaleOrdinal()
    .domain(["Bajo", "Medio", "Alto"])
    .range(["#DBC23E", "#F6812B", "#D92525"])

  let colorIntensidadmasOscuro = d3
    .scaleOrdinal()
    .domain(["Bajo", "Medio", "Alto"])
    .range(["#BCA00C", "#D86512", "#AE2727"])

  let colorGenero = d3.scaleOrdinal()
    .domain(["Rock", "Pop", "Tecno", "Folk", "Jazz", "Rap", "R&B", "Indie", "Urban", "Multivariado", "Pop,Rock", "R&B,Pop", "Rap,Jazz,Tecno"])
    .range(["gradient-rock", "gradient-pop", "gradient-tecno", "gradient-folk", "gradient-jazz", "gradient-rap", "gradient-rb", "gradient-indie", "gradient-urban", "gradient-multivariado", "gradient-poprock", "gradient-rbpop", "gradient-rapjazztecno"])

  let colorGeneromasOscuro = d3.scaleOrdinal()
    .domain(["Rock", "Pop", "Tecno", "Folk", "Jazz", "Rap", "R&B", "Indie", "Urban", "Multivariado", "Pop,Rock", "R&B,Pop", "Rap,Jazz,Tecno"])
    .range(["gradient-rock-o", "gradient-pop-o", "gradient-tecno-o", "gradient-folk-o", "gradient-jazz-o", "gradient-rap-o", "gradient-rb-o", "gradient-indie-o", "gradient-urban-o", "gradient-multivariado-o", "gradient-poprock-o", "gradient-rbpop-o", "gradient-rapjazztecno-o"])


    function calcularDuracion(frecuencia) {
    if (frecuencia === "Bajo") {
      return 10; // Duración más larga para frecuencia baja
    } else if (frecuencia === "Medio") {
      return 3; // Duración intermedia para frecuencia media
    } else {
      return 1; // Duración más corta para frecuencia alta
    }
  }

    onMount(() => {
    d3.csv("./data/UsodeMusica.csv", d3.autoType).then(data => {
      console.log(data)
      usuarios = data.map(usuario => {
        if (usuario.ArtistaBanda == "-") {
          return { ...usuario, opacidad: 0 }; // Si el nombre incluye "-", establece opacidad en 0
        } else {
          return { ...usuario, opacidad: 1 }; // Si no, establece opacidad en 1
        }
      });
    });
  });


</script>

<main>
  
  <div class="header">
    <img src="/images/play.gif" width="50" height="50" alt="play" />
    <h3 class="headline">
      <b>Gustos Musicales</b>
      Frecuencia, Género Musical, Artista y Likes
    </h3>
    <p class="bajada">Explorando los gustos musicales a través de datos</p>
  </div>
  

  <div class="container">
    {#each usuarios as usuario}
      <div class="person-container">
        <div class="corazones">
          <img src={`images/cora_${usuario.Cant_corazones}.png`} alt="Corazón" class="corazon" style="z-index: {8 - usuario.Cant_corazones}" /> </div>
        <div class="vinilo">
          <div class="girar {colorGenero(usuario.Genero)}" style="animation-duration: {calcularDuracion(usuario.Frecuencia)}s">
            <div class="semicirculo {colorGeneromasOscuro(usuario.Genero)}"></div>
            <div class="intensidad" style="background-color: {colorIntensidad(usuario.Frecuencia)}"></div> 
            <div class="semicirculointensidad" style="background-color: {colorIntensidadmasOscuro(usuario.Frecuencia)}"></div>
            <div class="punto"></div>
          </div>
          <div class="vinilo-overlay" style="opacity: {usuario.opacidad}"></div>
        </div>
        <a href={usuario.Link} target="_blank" rel="noopener noreferrer" p class="artista">
          <b>{usuario.ArtistaBanda}</b>
          <br />
          <span style="font-weight: lighter">{usuario.Nombre}</span>
        </a>
      </div>
    {/each}
  </div> 

  <div class="referencias">
    <p>Referencias</p>
    <img src={`images/referencias.svg`} width="1000" alt="referencias"/>
  </div>

</main>

<style>
  
  @keyframes rotate {
    from {
      transform: rotate(0deg); /* Inicia sin rotación */
    }
    to {
      transform: rotate(360deg); /* Termina con una rotación completa de 360 grados */
    }
  }

  .corazon {
    margin: 0px;
  }

  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .headline {
    font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
    font-size: 30px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin-bottom: 20px;
    margin-top: 0px;
  }

  .artista {
    text-align: center; /* Alinea el texto al centro */
    margin-top: 0px; /* Añade un espacio superior para separarlo del vinilo */
  }

  .bajada {
    font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
    font-size: 18px;
    font-weight: normal;
    text-align: center;
    margin: 20px;
  }

  .headline b {
    display: block;
  }

  .container {
    display: flex;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 1000px;
    gap: 30px;
    margin-bottom: 0px;
  }

  .person-container {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    flex: 180px 0 0;
    margin-top: 0px;
  }

  .girar {
    position: relative;
    width: 100px;
    height: 100px;
    border-radius: 50%;
    box-sizing: border-box;
    display: flex;
    justify-content: center;
    align-items: center; 
    animation: rotate 3s linear infinite; 
  }

  /* Estilos para los vinilos */
  .vinilo {
    position: relative;
    width: 100px;
    height: 100px;
    box-sizing: border-box;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 50px;
    
  }

  .referencias{
  font-family: Inter, system-ui, Avenir, Helvetica, Arial, sans-serif;
  font-style: normal;
  font-size: 30px;
  line-height: 1.2;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  right: 75%;
  }
  /* Estilos para la imagen SVG */

  .vinilo-overlay {
  position: absolute;
  width: 100px;
  height: 100px;
  background-image: url("/images/medalla.svg");
  background-size: cover; /* Ajusta el tamaño de la imagen */
  background-position: center 200px; /* Centra la imagen */
  opacity: 1; /* Opacidad completa */
  z-index: 3; /* Coloca la medalla encima del vinilo */
  top: 30px;
}
  .semicirculo {
    position: absolute;
    width: 50px; /* Ancho del semicírculo */
    height: 100px; /* Altura del vinilo */
    border-radius: 0 100px 100px 0; /* Borde redondeado */
    right: 25%; /* Posiciona el semicírculo en el centro horizontal del vinilo */
    top: 50%; /* Posiciona el semicírculo en el centro vertical del vinilo */
    transform: translate(50%, -50%); /* Centra el semicírculo respecto al vinilo */
  }

  /* Estilos para la intensidad */
  .intensidad {
    position: absolute; /* Posición absoluta */
    width: 50px;
    height: 50px;
    border: 10px solid black;
    border-radius: 50%;
    box-sizing: border-box;
    z-index: 1; /* Coloca el círculo de intensidad encima de los demás elementos */
  }

  .semicirculointensidad {
    position: absolute;
    width: 15px; /* Ancho del semicírculo */
    height: 30px; /* Altura del vinilo */
    border-radius: 0 50px 50px 0; /* Borde redondeado */
    right: 42.5%; /* Posiciona el semicírculo en el centro horizontal del vinilo */
    top: 50%; /* Posiciona el semicírculo en el centro vertical del vinilo */
    transform: translate(50%, -50%); /* Centra el semicírculo respecto al vinilo */
    z-index: 2; /* Coloca el semicírculo de intensidad encima del círculo de intensidad */
  }

  .punto {
  position: absolute;
  width: 5px; /* Tamaño del punto */
  height: 5px;
  background-color: black; /* Color del punto */
  border-radius: 50%; /* Para hacerlo circular */
  z-index: 5; /* Colócalo encima de los otros elementos */
}


/* Estilos adicionales según sea necesario */


  /* Estilos para los gradientes */
  .gradient-rock {
    background-color:  #0B984C;
  }

  .gradient-pop {
    background-color: #F5619F;
  }

  .gradient-tecno {
    background-color:  #8655D4;
  }

  .gradient-folk {
    background-color: #6BBA5E;
  }

  .gradient-jazz {
    background-color:  #7E83F8;
  }

  .gradient-rap {
    background-color: #BB51B7;
  }

  .gradient-rb {
    background-color:  #42B4BB;
  }

  .gradient-indie {
    background-color: #ADFF00;
  }

  .gradient-urban {
    background-color:  #F929A6;
  }

  .gradient-multivariado {
    background-image: linear-gradient(to bottom, #BB51B7, #7E83F8, #42B4BB, #6BBA5E, #D2D62A, #FF0505);
  }

  .gradient-poprock {
    background-image: linear-gradient(to bottom, #F5619F, #0B984C);
  }

  .gradient-rbpop {
    background-image: linear-gradient(to bottom, #42B4BB, #F5619F);
  }

  .gradient-rapjazztecno {
    background-image: linear-gradient(to bottom, #BB51B7, #8655D4, #7E83F8);
  }

  .gradient-rock-o {
    background-color:  #0C7B3F;
  }

  .gradient-pop-o {
    background-color: #D94382;
  }

  .gradient-tecno-o {
    background-color:  #724AB4;
  }

  .gradient-folk-o {
    background-color: #479546;
  }

  .gradient-jazz-o {
    background-color:  #6266CB;
  }

  .gradient-rap-o {
    background-color: #A12F9D;
  }

  .gradient-rb-o {
    background-color:  #399CA2;
  }

  .gradient-indie-o {
    background-color: #8BCB03;
  }

  .gradient-urban-o {
    background-color:  #D9148A;
  }

  .gradient-multivariado-o {
    background-image: linear-gradient(to bottom, #A12F9D, #6266CB, #399CA2, #479546, #B2B51D, #CE1D1D);
  }

  .gradient-poprock-o {
    background-image: linear-gradient(to bottom, #D94382, #0C7B3F);
  }

  .gradient-rbpop-o {
    background-image: linear-gradient(to bottom, #399CA2, #D94382);
  }

  .gradient-rapjazztecno-o {
    background-image: linear-gradient(to bottom, #A12F9D, #724AB4, #6266CB);
  }

</style>