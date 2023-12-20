<script>
    import "../../styles/singleCard.css"

    export let card
    export let imgCover
    export let handleChoice
    export let flipped
    export let disabled
    
    const handleClick = () => {
        if (!disabled) {
            handleChoice(card)	
        }	
    }
    </script>
    
    <div class="card">
        <div class:flipped="{flipped}">
            <img 
                src={card.src} 
                class="front" 
                alt="card front" />
            <img 
                src={imgCover} 
                class="back" 
                on:click={handleClick} 
                alt="card back" />
        </div>
    </div>
    
    
    