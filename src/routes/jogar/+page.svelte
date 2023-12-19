<script>
    import SingleCard from './SingleCard.svelte'
  
    let imgCover = './src/public/card-verso.png'
    let imgHelmet = './src/public/card-ruby.png'
    let imgPotion = './src/public/card-python.png'
    let imgRing = './src/public/card-php.png'
    let imgScroll = './src/public/card-js.png'
    let imgShield = './src/public/card-java.png'
    let imgSword = './src/public/card-c++.png'
  
    const cardImages = [
      { src: imgHelmet, matched: false },
      { src: imgPotion, matched: false },
      { src: imgRing, matched: false },
      { src: imgScroll, matched: false },
      { src: imgShield, matched: false },
      { src: imgSword, matched: false },
    ]
  
    let cards = []
    let turns = 0
    let choiceOne = null
    let choiceTwo = null
    let disabled = false
  
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
  </script>
  
  <div class="App">
    <h1>Magic Match</h1>
    <button on:click={shuffledCards}>
      Novo Jogo
    </button>
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
            card.matched} />
      {/each}
    </div>
    <p>
      Turnos: {turns}
    </p>
  </div>

  <style>
    .App {
      max-width: 860px;
      margin: 0 auto;
        min-height: 100vh;
        text-align:center;
        padding: 1rem;
        color: black;
    }
        h1 {
            color: black;
        }
    button {
      background: none;
      border: 2px solid #fff;
      padding: 6px 12px;
      border-radius: 4px;
      color: black;
      font-weight: bold;
      cursor: pointer;
      font-size: 1em;
    }
    button:hover {
      background: #c23866;
      color: black;
    }
    .card-grid {
        margin-top: 40px;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-gap: 20px;
    }
    </style>