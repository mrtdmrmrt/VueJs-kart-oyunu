<template>
	<div class="game-area">
		<h1 class="title">Yemeğim <span>Nerede </span><strong>?</strong></h1>
		<h4 class="description">Açık kartlardan birini seçtikten sonra , kapalı olan karta tıklayınız</h4>
		<div class="container">
			<!--appear sayfa yenilendiğinde hemen çalışır-->
			<transition-group name="rotateAll" appear class="container">
				<app-card 
					:key="card.id"
					:class="{'shadow' : selectedCard == card.id}"
					@click.native="selectedCard=card.id"
					v-for="card in cards" 
					:card="card">
				
				</app-card>
			</transition-group>
			
		</div>
		<div class="container">
			<transition name="rotate" mode="out-in">
				<component
				@click.native="showCard(answer)"
				:card="answer"
				:is="activeCard">
				
				</component>
			</transition>
			
			
		</div>
	</div>
</template>

<script>
	import Card from "./Card";
	import DefaultCard from "./DefaultCard";
	export default{
		data(){
			return {
				selectedCard : null,
				answer: {},
				activeCard : "app-default-card",
				cards : [
				{id : 1, component : "app-card" , image : "/src/assets/card-1.jpg" },
				{id : 2, component : "app-card" , image : "/src/assets/card-2.jpg" },
				{id : 3, component : "app-card" , image : "/src/assets/card-3.jpg" },
				{id : 4, component : "app-card" , image : "/src/assets/card-4.jpg" },
				{id : 5, component : "app-card" , image : "/src/assets/card-5.jpg" }
				]
			}
			console.log(answer);
		},
		created(){
			let answer = Math.ceil(Math.random() * this.cards.length);
			
			this.answer=this.cards[answer-1];
			console.log(this.answer);
		},
		components : {
			appCard : Card,
			appDefaultCard : DefaultCard
		},
		methods : {
			showCard(answer){
				if(this.selectedCard==null){
					alert("İlk Olarak Kart Seçiniz..")
				}
				else{
					this.activeCard = answer.component;
					setTimeout(()=>{
						if(answer.id == this.selectedCard){
							//alert("Doğru");
							this.$emit("activeComponentEvent", "app-celebrate");
						}
						else{
							//alert("Yanlış");
							this.$emit("activeComponentEvent", "app-failure");
						}
					},2500)


					
					
				}
				
			}
		}
	}
</script>

<style scoped>

	.title {
		text-align: center;
		
		color : rosybrown;
	}
	.title span{
		color:mediumpurple;
	}
	.title strong{
		color: darkred;
	}

	.description {
		color:grey;
		text-align: center;
	}

	.container{
		display: flex;
		justify-content: center;
		align-items: center;
		margin-top: 50px;

	}

	.shadow{
		box-shadow: 0px 5px 48px #30969f!important;
		transition: box-shadow .5s;
	}

	/*----------------Açık kartlar için animasyonun yapıldığı yer----------------*/

	.rotateAll-enter{}
	.rotateAll-enter-active{
		animation: rotateAll ease-in-out 2s forwards;
	}
	.rotateAll-leave{}
	.rotateAll-leave-active{}

	/*ilk durum belirttiğimiz yer*/
	@keyframes rotateAll {
		from{
			transform: rotateY();
		}
		to{
			transform: rotateY(720deg);
		}
	}

	/*----------------Kapalı kartlar için animasyonun yapıldığı yer----------------*/
	.rotate-enter{}
	.rotate-enter-active{
		animation: rotate-in 1s ease-in-out forwards;
	}
	.rotate-leave{}
	.rotate-leave-active{
		animation: rotate-out 1s ease-in-out forwards;
	}

	@keyframes rotate-in{
		from{
			transform: rotateY(90deg);
		}
		to{
			transform: rotateY(0deg);
		}
	}
	@keyframes rotate-out{
		from{
			transform: rotateY(0deg);
		}
		to{
			transform: rotateY(90deg);
		}
	}
	
</style>