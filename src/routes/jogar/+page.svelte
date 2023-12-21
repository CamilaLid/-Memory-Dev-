<script>
    import SingleCard from './SingleCard.svelte'
    import "../../styles/play.css"
  
    let imgCover = './src/public/card-verso.png'
    let imgRuby = './src/public/card-ruby.png'
    let imgPython = './src/public/card-python.png'
    let imgPhp = './src/public/card-php.png'
    let imgJs = './src/public/card-js.png'
    let imgJava = './src/public/card-java.png'
    let imgCplus = './src/public/card-c++.png'
    let gameComplete = false;


    const cardImages = [
      { src: imgRuby, matched: false },
      { src: imgPython, matched: false },
      { src: imgPhp, matched: false },
      { src: imgJs, matched: false },
      { src: imgJava, matched: false },
      { src: imgCplus, matched: false },
    ]
  
    let cards = []
    let turns = 0
    let choiceOne = null
    let choiceTwo = null
    let disabled = false

    // Volta para tela inicial
    const backToMenu = () => {
      
      window.location.href ="/"
    }
    // Embaralhar as cartas
    const shuffledCards = () => {
      const shuffledCards = [...cardImages, ...cardImages]
        .sort(() => Math.random() - 0.5)
        .map((card) => ({ ...card, id: Math.random() }))
      cards = shuffledCards
      turns = 0
    }
  
    // Lidar com uma escolha
    const handleChoice = card => {
      if (choiceOne && choiceOne.id === card.id){
      return 
      }
      choiceOne ? choiceTwo = card : choiceOne = card
    }


    // Comparar 2 cartas selecionadas
    $: if (choiceOne && choiceTwo) {
      disabled = true
      if (choiceOne.src === choiceTwo.src) {
        console.log('essas cartas combinam')
        cards = cards.map(card => {
          if (card.src === choiceOne.src) {
            return { ...card, matched: true }
          } else {
            return card
          }
        })
        resetTurn()
      } else {
        console.log('essas cartas não combinam')
        setTimeout(() => resetTurn(), 1000)
      }
    }
  
    // Iniciar um jogo automaticamente
    $: shuffledCards()
  
    // Redefinir escolhas e aumentar turno
    const resetTurn = () => {
      choiceOne = null
      choiceTwo = null
      turns = turns + 1
      disabled = false
    }

   
// Verificar se as cartas estao viradas
$: {
    if (cards.every(card => card.matched)) {
        gameComplete = true;
        const mainContent = document.querySelector(".mainContent");
        if (mainContent) {
          mainContent.classList.remove("mainContent");
          mainContent.classList.add("darken")
        }
    }
}

//  reiniciar o jogo depois que acabar
const restartGame = () => {
    shuffledCards();
    gameComplete = false;
    const mainContent = document.querySelector(".darken");
      if (mainContent) {
        mainContent.classList.remove("darken");
        mainContent.classList.add("mainContent");
      }
  }


</script>
<div class="mainContent">
<div class="header-play">
  <button on:click={backToMenu}>
      <input class="btn" type="image" src="src\public\botao-voltar.png" alt="back">
  </button>
  <div>
      <img src="src\public\logo-jogar.png" alt="logo"/>
      <p class="turns">
          Turns = {turns}
      </p>
  </div>
  <button on:click={shuffledCards}>
      <input class="btn" type="image" src="src\public\botao-reiniciar.png" alt="restart"> 
  </button>

  </div>
<div class="App"> 
  <div class="card-grid">
      {#each cards as card (card.id)}
          <SingleCard
              {card}
              {imgCover}
              {disabled}
              {handleChoice}
              flipped={
                  card === choiceOne ||
                  card === choiceTwo ||
                  card.matched
              }/> 
      {/each}
  </div>
</div>
</div>
{#if gameComplete}
 <!-- Tela de FINAL -->
 <div class="game-complete">
  <h1 class="text">Congratulations!</h1> 
  <p class="text1">You found all the pairs in:</p>
  <h2 class="text2"> {turns} Turns </h2>
  <button class="button-end" on:click={restartGame}>PLAY AGAIN</button>
  <a href="/" ><button class="button-end">MENU</button></a>
</div>
{/if}