<template>
    <div id="game_area">
        <div id="game_panel">
            <div class="game_panel_inner">
                <div v-if="player === ''">
                    <h1 class="game_panel_heading">Choose Your Hand</h1>
                    <div class="select_hand">
                        <button @click="player = 'X'" type="button" class="button select_hand_button">
                            X
                        </button>
                        <button @click="player = 'O'" type="button" class="button select_hand_button">
                            O
                        </button>
                    </div>
                </div> 
                <div v-if="player !== '' && winner === ''">
                    <h1 class="game_panel_heading">Let's play!</h1>
                    <div class="game_content">
                        <div class="row" v-for="(row, index) in xox" v-bind:key="index">
                            <a class="column" @click="hand(index, 0)">{{ row[0] }}</a>
                            <a class="column" @click="hand(index, 1)">{{ row[1] }}</a>
                            <a class="column" @click="hand(index, 2)">{{ row[2] }}</a>
                        </div>
                    </div>
                    <div class="game_buttons">
                        <button type="button" id="restartButton" class="button button-refresh" @click="restartGame(1)">
                            <i class="icon ion-refresh"></i>
                            <span>Restart Game</span>
                        </button>
                    </div>
                </div>
                <div v-if="winner != ''">
                    <h1>Winner is <span class="font-indie">{{ winner }}</span>!</h1>
                    <div class="game_buttons">
                        <button type="button" @click="restartGame(0)" class="button button-refresh">
                            <i class="icon ion-refresh"></i>
                            <span>Play Again</span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Game',
    data(){
        return {
            xox: [
                [ '', '', '' ],
                [ '', '', '' ],
                [ '', '', '' ]
            ],
            player: '',
            winner: '',
            turn: 'player'
        }
    },
    methods: {
        hand: function(x, y){
            if(this.turn === 'player' && this.xox[x][y] == ''){
                this.xox[x][y] = this.player;
                this.getWinner();
                this.turn = 'computer';
                setTimeout(() => {
                    this.playComputer();
                    this.getWinner();
                },700);
                if(this.gameCompleted() === true){
                    if(this.winner === ''){
                        this.winner = 'nothing';
                    }
                }
                this.$forceUpdate();
            }
        },
        playComputer: function(){
            while(true){
                var rndX = Math.floor(3 * Math.random());
                var rndY = Math.floor(3 * Math.random());
                if (this.xox[rndX][rndY] === ''){
                    this.xox[rndX][rndY] = (this.player === 'X' ? 'O' : 'X');
                    this.turn = 'player';
                    this.$forceUpdate();
                    break;
                }
            }
        },
        getWinner: function(){
            /*  
                X
                    X
                        X
            */
            if(this.xox[0][0] === this.xox[1][1] && this.xox[1][1] === this.xox[2][2]){
                this.winner = this.xox[0][0];
                return;
            }
            /*  
                        X
                    X
                X
            */
            if(this.xox[0][2] === this.xox[1][1] && this.xox[1][1] === this.xox[2][0]){
                this.winner = this.xox[0][2];
                return;
            }
            /* 
                X   X   X
            */
            for(var a = 0; a < 3; a++){
                if(this.xox[a][0] === this.xox[a][1] && this.xox[a][1] === this.xox[a][2]){
                    this.winner = this.xox[a][0];
                    break;
                    return;
                }
            }
            /* 
                X
                X
                X
            */
            if(this.xox[0][0] === this.xox[1][0] && this.xox[1][0] === this.xox[2][0]){
                this.winner = this.xox[0][0];
                return;
            }
            if(this.xox[0][1] === this.xox[1][1] && this.xox[1][1] === this.xox[2][1]){
                this.winner = this.xox[0][1];
                return;
            }
            if(this.xox[0][2] === this.xox[1][2] && this.xox[1][2] === this.xox[2][2]){
                this.winner = this.xox[0][2];
                return;
            }
        },
        gameCompleted: function(){
            var null_count = 0;
            for(var b = 0; b < this.xox.length; b++){
                for(var c = 0; c < this.xox[b].length; c++){
                    if(this.xox[b][c] === ''){
                        null_count++;
                    }
                }
            }
            if(null_count > 0){
                return false;
            }else{
                return true;
            }
        },
        restartGame: function(x){
            x == 1 ? document.querySelector("#restartButton span").innerHTML = "Restarting..." : null;
            setTimeout(()=>{
                this.player = '';
                this.turn = 'player';
                this.winner = '';
                for(var a = 0; a < this.xox.length; a++){
                    for(var b = 0; b < this.xox[a].length; b++){
                        this.xox[a][b] = '';
                    }
                }
                console.log("Game restarted.");
                this.$forceUpdate();
                x == 1 ? document.querySelector("#restartButton span").innerHTML = "Restart Game" : null;
            }, (x == 1 ? 1000 : 0) );
        }
    }
}
</script>

<style lang="scss" scoped>
#game_area{
    background:transparent;
    display:table;
    width:100%;
    height:100%;
}
#game_panel{
    display:table-cell;
    vertical-align:middle;
    .game_panel_inner{
        width:400px;
        padding:10px;
        margin:auto;
        border-radius:5px;
        background:black;
        h1{
            color:white;
            text-align:center;
        }
        .select_hand{
            display:flex;
            .select_hand_button{
                font-family: 'Indie Flower', cursive;
                font-size:2rem;
                width:50%;
                padding:10px 20px;
                background-color:#34359b;
                color:white;
                border:0;
                transition: all 0.3s ease;
            }
            .select_hand_button:hover{
                background-color:#0f5a9c;
            }
            .select_hand_button:first-child{
                margin-right:1%;
            }
        }
        .game_panel_heading{
            font-family: 'Questrial', sans-serif;
            color:white;
            text-align:center;
            margin:0 0 1rem 0;
        }
        .game_content{
            margin:auto;
            padding:.5rem;
            width:200px;
            .row{
                display:flex;
                flex-direction: row;
                border-bottom:2px solid #333;
                .column{
                    display:inline-block;
                    padding:1rem;
                    width:33.33333333333%;
                    height:80px;
                    font-size:2rem;
                    color:white;
                    text-decoration:none;
                    text-align:center;
                    cursor:pointer;
                    font-family: 'Indie Flower', cursive;
                }
                .column.p1{
                    color:#7a88f9;
                }
                .column.p2{
                    color:#e53935;
                }
                .column:first-child,
                .column:nth-child(2){
                    border-right:2px solid #333;
                }
            }
            .row:last-child{
                border-bottom:0;
            }
        }
        .game_buttons{
            display:flex;
            flex-direction: row;
            padding-top:1rem;
            width:100%;
            justify-content: center;
        }
        .font-indie{
            font-family: 'Indie Flower', cursive;
        }
    }
}
.button{
    cursor:pointer;
    display:inline-block;
    border:0;
    padding:10px 20px;
}
.button.button-refresh{
    background-color:#34359b;
    color:white;
    transition: all 0.3s ease;
}
.button-refresh:hover{
    background-color:#0f5a9c;
}
</style>