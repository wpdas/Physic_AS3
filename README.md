![alt tag](https://github.com/Wpdas/Physic_AS3/blob/master/docs/billard-gl.png)
# Physic AS3 v1.1.4
>Author: Wenderson Pires - wpdas@yahoo.com.br

Physic é uma Framework para simulação de física. Foi construída para ser usada em jogos simples que necessitam de ações simples naturalizadas da física.
É compatível com AIR Mobile (Android e iOS) e AIR Desktop.

#### Principais Características:

- Simular Gravidade;
- Simular Resistência;
- Simular Força;
- Simular Força Resultante;
- Simular Colisão;
- Simular Impulsão;
- Compatibilidade com Starling.

#### Dados disponíveis para estudo:
| Tipo | Descrição |
| --- | --- |
| Documentação | [Documentação completa da biblioteca v1.0.0](https://rawgit.com/Wpdas/Physic_AS3/master/docs/index.html). |
| Arquivo de Exemplo | [Arquivos prontos para teste (projeto .fla) v1.0.0](https://github.com/Wpdas/Physic_AS3/tree/master/example). |

## Como Usar
Baixe este exemplo abaixo para ver o funcionamento: [Example Files v1.0.0](https://github.com/Wpdas/Physic_AS3/tree/master/example).

```actionscript
package  {
	
	import com.physic.Gravity;
	import com.physic.body.RigidBody;
	import com.physic.body.StaticBody;
	import com.physic.event.BodyEvent;
	import com.physic.pivot.AddPivot;
	import com.physic.pivot.Pivot;
	import com.physic.pivot.PivotRegister;
	import com.physic.display.SpritePhysic;
	import flash.display.Sprite;
	import flash.events.Event;
	
	/**
	* Exemplo de uso da biblioteca
	* @version 1.0.0
	*/
	public class Main extends Sprite {
		
		//Física
		private var gravity:Gravity;
		private var rocketBody:RigidBody;
		private var meteorBody:RigidBody;
		private var planetOneBody:RigidBody;
		private var planetTwoBody:RigidBody;
		private var groundStaticBody:StaticBody;
		
		//Elementos (Devem ser setados na base da biblioteca caso sejam instanciadas lá)
		private var rocket:SpritePhysic; //Só é suportado este tipo de objeto (necessário para tratar Pivot, ponto de registro do objeto)
		private var meteor:SpritePhysic;
		private var planet1:SpritePhysic;
		private var planet2:SpritePhysic;
		private var ground:SpritePhysic;
		
		public function Main() {
			
			//Cria elementos
			ground = new Ground();
			ground.updatePivot(Pivot.TOP_CENTER);
			ground.x = 265;
			ground.y = 729;
			addChild(ground);
			
			rocket = new Rocket();
			rocket.updatePivot(Pivot.CENTER); //Atualiza Pivot (Ponto de registro do objeto)
			rocket.x = 636;
			rocket.y = 623;
			addChild(rocket);
			
			planet1 = new PlanetOne;
			planet1.updatePivot(Pivot.CENTER);
			planet1.x = 117;
			planet1.y = 148;
			addChild(planet1);
			
			meteor = new Meteor();
			meteor.updatePivot(Pivot.MANUAL_POINT(meteor.width/2, 120));
			meteor.x = 905.50;
			meteor.y = 184.60;
			addChild(meteor);
			
			planet2 = new PlanetTwo();
			planet2.x = 1040;
			planet2.y = 122;
			addChild(planet2);
			
			//Aplica fisica
			//Força da Física Gravitacional
			gravity = new Gravity(5);
			//gravity.gravity = 15; //(!) pode ser alterada a força de atração gravitacional
			
			//Elementos a qual a Física gravitacional interage
			groundStaticBody = new StaticBody(ground);
			gravity.insertBody(groundStaticBody);
			
			rocketBody = new RigidBody(rocket, 1);
			rocketBody.isDown = false;
			rocketBody.addEventListener(BodyEvent.ON_TAP_CLICK, ignition); //Aciona ignicao no foguete
			rocketBody.addEventListener(BodyEvent.ON_MOVE_UP, onRocketMoveUp);
			rocketBody.addEventListener(BodyEvent.ON_BODY_STOP, onRocketMoveUp);
			rocketBody.addEventListener(BodyEvent.ON_MOVE_DOWN, onRocketMoveDown);
			gravity.insertBody(rocketBody);
			
			planetOneBody = new RigidBody(planet1, 3, true);
			gravity.insertBody(planetOneBody);
			planetOneBody.addHorizontalForce(10);
			
			planetTwoBody = new RigidBody(planet2, 1);
			gravity.insertBody(planetTwoBody);
			
			meteorBody = new RigidBody(meteor, 2);
			gravity.insertBody(meteorBody);
			
			
			//Ativa renderizador
			addEventListener(Event.ENTER_FRAME, render);
			
			//(!)Basta remover o evento para parar a renderização da gravidade
			//removeEventListener(Event.ENTER_FRAME, render);
		}

		//Altera escala do Foguete
		private function onRocketMoveUp(e:BodyEvent):void {
			
			rocketBody.body.pivot.scaleY = 1;
		}
		
		//Altera escala do Foguete
		private function onRocketMoveDown(e:BodyEvent):void {
			
			rocketBody.body.pivot.scaleY = -1;
		}
		
		//Aciona ignição no foguete
		private function ignition(e:BodyEvent):void {
			
			//e.target.addVerticalForce(-15); (!) Também funciona
			rocketBody.addVerticalForce(-15);
			
			//Remove corpo da força gravitacional
			gravity.removeBody(planetOneBody);
		}
		
		//Renderiza
		private function render(e:Event):void {
			
			gravity.render();
		}
	}
}

```

Log de Alterações (changelog)
-----
####version 1.1.4 - 2016-08-14
- Novo detector de evento
- BodyEvent.ON_INTERSECTS usado para detectar quando um objeto entrar dentro do outro

####version 1.1.3 - 2016-08-08
- Novo detector de evento
- BodyEvent.ON_COLLISION usado para detectar quando um objeto colidir com outro

####version 1.1.2 - 2016-08-01
- Implementação aprimorada de colisão de objetos Starling

####version 1.1.1 - 2016-08-01
- Inserção de compatibilidade com Starling
- Correção estrutural do método "removeBody()" do elemento Gravity

####version 1.0.0 - 2016-07-28
- Primeira versão publicada


License
----

MIT
