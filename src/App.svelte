<script>
	import About from './About.svelte';
	import Nav from './Nav.svelte';
	import Footer from './Footer.svelte';
	import Social from './Social.svelte';
	import Portfolio from './Portfolio-OLDVERSION.svelte';
	import { onMount } from 'svelte';
	import { watchResize } from "svelte-watch-resize";
	// import axios from 'axios';



	//local vars
	let reso;
	let hideMenu=true;
	let modalMenu;
	let mainRef;
	let modalMessage;
	let message;
	let email;
	//this var set to setContext for studie propousal

	//imports from childs (in childs same name but "export" specification)
	let showAbout=true;
	let showPortfolio;
	let socialSide;
	let canvasSocialSide;
	let skillsEl;
	let whoEl;

	onMount(async()=>{

		// const { data } = await axios.get('http://localhost:4000/api');
    	// console.log(data.emojis);

		let vh = window.innerHeight * 0.01;
		document.documentElement.style.setProperty('--vh', `${vh}px`);
		
		mainRef.addEventListener('change', ()=>{
		});

		reso = window.innerWidth;
		window.addEventListener("mousemove",handleMouseOutside);
		// console.dir(modalMenu);
		if(reso<640){
			mainRef.style.setProperty("height", "auto");
		}
		hideMenu=true; //for example this modified the hideMenu var original from the child exports it		
	});



	const resize = ()=>{
		reso = window.innerWidth;

		if(reso<640){
			mainRef.style.setProperty("height", "auto !important");
		}
		if(reso>640){
		}
	}

	//watch the mouse move for detect if is out or inside the element we pass it
	const matchElementEvent = (element, event)=>{
		let x = parseInt(event.clientX);
		let y = parseInt(event.clientY);
		// console.log("mouse x: "+x," mouse y: "+y);

		let top = parseInt(element.offsetTop);
		let bot = parseInt(element.offsetTop+element.offsetHeight);
		let left = parseInt(element.offsetLeft);
		let right = parseInt(element.offsetLeft+element.offsetWidth);
		// console.log("bot: "+bot+" top: "+top);
		// console.log("left: "+left+" right: "+right);

		if(y>bot || x>right || y<top || x<left){
			// console.log("hide element...");
			// console.log(true);
			// document.removeEventListener("mousedown", handleClickOutside);
			return true;	
		}
		else{
			// console.log("dont hide element...");
			// document.removeEventListener("mousedown", handleClickOutside);
			return false;
		}
	}

	//with helps from the function "matchElementEvent"
	//i setup here whats elements will be watching
	const handleMouseOutside = async(event)=> {
		if(hideMenu===false){
			if(matchElementEvent(modalMenu,event)){
				 hideMenu=true;
			}
			else{hideMenu=false};//is necesary?
		}

			if(matchElementEvent(canvasSocialSide,event)){
				socialSide.classList.remove("social-over");
			}
			else{
				socialSide.classList.add("social-over");
			}
	}  
	
	const clickMenu = ()=>{
		console.log("cliii");
            hideMenu=false;  
	}
	
	const handleButton = ()=>{
		let el = modalMessage;
        el.style.setProperty('display','flex');
        el.style.setProperty('opacity','100%');
        document.querySelector('.message-background').style.setProperty("opacity","80%");
        document.querySelector('.message-background').style.setProperty("z-index","1000000");
        el.classList.remove("animateMessage");
        void el.offsetWidth; // trigger a DOM reflow //taken from stackoverflow
		el.classList.add("animateMessage");
		hideMenu=true;
	}
	const closeModalMessage = ()=>{
		let el = modalMessage
		el.style.setProperty('display','none');
        el.style.setProperty('opacity','0%');
        document.querySelector('.message-background').style.setProperty("opacity","0%");
        document.querySelector('.message-background').style.setProperty("z-index","1");
	}
	
	const sendemail = ()=>{
		alert("mail");

	}
	const cleanButton = ()=>{
		email.value = "";
		message.value = "";
	}

</script>
<div class="message-background" on:click={closeModalMessage}>
	
</div>
<div class="message animateMessage" bind:this={modalMessage}>
		<h1 style="cursor:pointer;position:absolute;right:0;top:-15%" on:click={closeModalMessage}>X</h1>

		<div style="height:100%;width:75%;display:flex;flex-direction:column;justify-content:center;justify-items:center;align-items:center;align-content:center;">

			<div class="your-name"style="width:80%;height:30%;display:flex;flex-direction:column;justify-content:center;justify-items:center;align-items:center;align-content:center;">
				<h5 style="padding:0;margin:0;">Your name/email</h5>
				<input id="email" placeholder="type here..." bind:this={email} style="text-align:center;width:90%;height:auto">
			</div>
	
			<div class="your-message" style="width:80%;height:60%;display:flex;flex-direction:column;align-items:center;">
				<h5 style="padding:0;margin:0;">Your Message</h5>
				<textarea id="mensaje" bind:this={message} style="height:70%;width:90%;"></textarea>
			</div>
		
			<div class="buttons" style="width:80%;height:auto;width:30%;display:flex;flex-direction:row;justify-content:space-evenly !important;" >   
				<button target="_blank" id="sendemail" on:click={sendemail} style="color:rgb(0, 0, 0);background-color: chartreuse;">ENVIAR</button>
				<button on:click={cleanButton} id="clean" style="color:white;background-color: rgba(216, 0, 0, 0.568);">LIMPIAR</button>
			</div>
				
		</div>

		<div class="contact-phrase" style="font-size:1.7vw">
			<h2>Ponete en contacto de la forma mas facil!</h2>
			<h3>Esperamos tu consulta...</h3>
		</div>
				
</div>

{#if (!hideMenu)}
<div bind:this={modalMenu} class="menu-modal">
	<h4 class="menu-modal-item" style="color:white;background-color:black">Sergio David Posse</h4>
	<h6 class="menu-modal-item" style="margin-top:-10%;color:white;background-color:weat;">Software Developer</h6>
	<h5 class="email" style="color:white;cursor:text !important;padding:3%;">Sergiodavidposse@gmail.com</h5>
	{#if reso<640}
		<span>
			<Social></Social>
		</span>
	{/if}
    <button on:click={handleButton} class="quick-button">Quick message</button>
</div>
{/if}

<main bind:this={mainRef} use:watchResize={resize} >

	<img on:click={clickMenu} src="/images/menu.png" alt="menu"/>

	<Nav bind:showAbout={showAbout} bind:showPortfolio={showPortfolio}></Nav>
	{#if reso>	640}
		<Social bind:canvasSocialSide={canvasSocialSide} bind:socialSide={socialSide}></Social>
	{/if}

	{#if (showAbout)}
		<About bind:skillsEl={ skillsEl } bind:whoEl={ whoEl }></About>
	{/if}

	{#if (!showAbout)}
		<Portfolio></Portfolio>
	{/if}
	
	<Footer></Footer>
</main>

<style>
#mensaje:focus,#email:focus{
	background-color:var(--dark-beta);
	color:var(--m-purple);
	border:1px solid var(--h-purple);
}
.animateMessage{
    animation: bringoverMessage 0.5s cubic-bezier(0,1,.37,.32) forwards;
}
@keyframes bringoverMessage{
        0%{
            transform:translateX(0%);
            opacity:20%;
        }
        80%{ transform:translateX(50%);}
        85%{
            transform:translateX(47%);
        }
        90%{
            transform:translateX(50%);
        }
        95%{
            transform:translateX(47%);
        }
        100%{
            transform:translateX(50%);
            opacity:100%;
        }
    }
.message-background{
    position:absolute;
    left:0;
    top:0;
    width:100vw;
    height:100vh;
    background-color:black;
    opacity:0%;
}
.message{
	color:var(--l-purple);
    background: linear-gradient(90deg, var(--h-purple) 20%, var(--dark) 89%);
    position:absolute;
    display:none;
    padding:1rem;
    flex-direction:row-reverse;
    width:50%;
    top:15%;
    height:50%;
    z-index:66663936 !important;
    margin:auto;
    font-size: 1.7vw;
    opacity:100% !important	;
    justify-content:center;
    justify-items:center;
    align-items:center;
	align-content:center;
	border-radius:20px;
}
.message h1{
	position:absolute;
	right:0;
	top:0;
}
	.quick-button ,.quick-button:hover{
		behavior: inherit;
		cursor:pointer;
		width:80%;
		display:flex;
		align-self:center;
		justify-content: center;
		border:1px solid rgb(35, 34, 43);
		border-radius:25px;
		padding:2%;
		color:var(--l-purple);
		box-shadow:0px 1px 6px var(--dark);
		background-color:var(--main-purple);
		outline:none;
		z-index:5000;
	}
	.quick-button:focus:hover{
		background-color:var(--h-purple);
		border:none;
		user-select: none;
		box-shadow:0px 1px 6px rgb(37, 34, 34);
		outline:none;
	}

	img{
        width:4%;
        height:8%;
        padding:2%;
		top:0% !important;
		left:0;
        filter:invert();
        cursor:pointer;
		position:fixed;
		z-index:11000;
		object-fit: scale-down;
    }
	main {
		background-color:var(--dark-alpha);
		width:100vw;
		max-width: 100%;
		height:100vh;
		max-height:100%;
		text-align: center;
		margin:0;
		padding:0 !important;
		display:flex;
		flex-direction:column;
		z-index:600;
		overflow:hidden;
	}
	main::before {
			content: "";
			position: absolute;
			width: 100%;
			height: 100%;
			top: 0;
			left: 0;
			z-index: -1;
			background-image: url('/images/galaxy-big.jpg');
			background-size: cover;
			background-position: center center;
			background-repeat: no-repeat;
			animation: backfloat 7s ease infinite alternate !important;	
		}
	@keyframes backfloat{
		0%{
			transform: scale(1,1);
		}
		100%{
			transform: scale(1.5,1.5);
		}
	}
	@keyframes backfloat2{
		0%{
			transform: scale(1,1);
		}
		100%{
			transform: scale(1.7,1.7);
		}
	}
	.menu-modal{
        position: absolute;
        left:0;
        top:0;
        display:flex;
        flex-direction:column;
        justify-content: space-evenly;
        height:50%;
        width:20%;
        background-color:var(--dark);
        z-index: 34500 !important;
        color:var(--dark);
        filter: drop-shadow(16px 16px 20px var(--l-purple));
		text-align: center;
		padding:2%;
    }
	.menu-modal:before{
		position:absolute;
		border:1px solid var(--h-purple);
		width:90%;
		height:90%;
		display:flex;
		align-self:center;
		align-items: center;
		align-content:center;
		justify-content: center;
		justify-items:center;
		content:"";
	}
    .menu-modal span{
        cursor:pointer;
        display:flex;
        justify-content: center;
        align-content:center;
        align-items:center;
        text-align: center;
        height:20%;
    }
    .menu-modal .email{
		background: linear-gradient(14deg, var(--main-purple-beta) 10%, var(--h-purple) 79%);
		color:var(--l-purple) !important;
		cursor:text !important;
		font-size:1vw;
		z-index:9999999;
		user-select: text;
    }
	h4{
        opacity: 0.4;
	}
	@keyframes bringoversmallMessage{
        0%{
            transform:translateY(-500%);
            opacity:20%;
        }
        100%{
            transform:translateY(0%);
            opacity:100%;
        }
    }
	@media (max-width: 640px) {
		.animateMessage{
    animation: bringoversmallMessage 0.5s cubic-bezier(0,1,.37,.32) forwards;
}
		main::before {
			content: "";
			position: absolute;
			width: 100%;
			height: 100%;
			top: 0;
			left: 0;
			z-index: -1;
			background-image: url('/images/the-night-sky-2401702_1920.jpg');
			background-size: cover;
			background-position: center center;
			background-repeat: no-repeat;
			animation: backfloat2 6s ease-in-out infinite alternate !important;	
		}
		main {
			overflow:hidden;
			width:100vw;
			max-width:100% !important;
			left:0;
			height:auto;
			background-image: none;
		}
		.menu-modal{
			height: calc(var(--vh, 1vh) * 70);
			font-size: calc(var(--vh, 1vh) * 3);
			padding: calc(var(--vh, 1vh)* 1);
            width:80% !important;
            border-radius:0 0 0 0;

        }
		img{
			position:fixed;
			top:7vh !important;
			left:1vw;
            width:13vw;
			height:8vh;
			z-index:110000;
		}
		.menu-modal .email{
			width:80%;
			display:flex;
			align-self:center;
			text-align:center;
			font-size:90%;
		}
		.message{
			flex-direction:column;
			font-size:100%;
			left:12.5%;
			height:70%;
			align-self:center !important;
			justify-self: center !important;
			width:75%;
			padding:0;
		}
		.your-message,.your-name,.buttons,.contact-phrase{
			width:90%;
		}
		.buttons{
			width:100%;
			justify-content: space-between;
			height:10% !important;
			padding:0 !important;
		}
		.buttons button{
			width:auto !important;
		}
		.contact-phrase{
			font-size:70% !important;
		}
	}
</style>