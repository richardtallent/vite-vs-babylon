<template>
	<canvas style="width: 720px; height: 480px; background-color: black"></canvas>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import { Engine } from "@babylonjs/core/Engines/engine";
import { Scene } from "@babylonjs/core/scene";
import { UniversalCamera } from "@babylonjs/core/Cameras/universalCamera";
import { HemisphericLight } from "@babylonjs/core/Lights/hemisphericLight";
import { Vector3 } from "@babylonjs/core/Maths/math.vector";
import { BoxBuilder } from "@babylonjs/core/Meshes/Builders/boxBuilder";
import { StandardMaterial } from "@babylonjs/core/Materials/standardMaterial";
import { Color3 } from "@babylonjs/core/Maths/math.color";
import { PhotoDome } from "@babylonjs/core/Helpers/photoDome";

// This helped with a similar import issue in Vite < b24, but is no longer
// the secret sauce and has no impact.
import "@babylonjs/core/Helpers/sceneHelpers";

let canvas, engine, scene, camera;

export default defineComponent({
	name: "App",
	mounted() {
		canvas = document.getElementById("renderCanvas") as HTMLCanvasElement;
		engine = new Engine(canvas, true, {}, true);
		scene = new Scene(engine);
		camera = new UniversalCamera("cameraNormal", new Vector3(0, 2, -10), scene);

		// First problem here. Unless I clear inputs first and re-add the ones I want specifically,
		// I get an onGamepadConnectedObservable error (I don't have or use gamepads).
		// camera.inputs.clear();
		// camera.inputs.addMouse();
		// camera.inputs.addKeyboard();

		scene.activeCamera = camera;
		camera.attachControl(canvas);

		new HemisphericLight("light", new Vector3(0, 1, 0), scene);

		// Second problem here.
		// Cannot read property 'instancedArrays' of undefined
		const box = BoxBuilder.CreateBox("example", { size: 1 }, scene);
		const boxMat = new StandardMaterial("boxmaterial", scene);
		box.material = boxMat;
		boxMat.diffuseColor = Color3.Red();

		// Third problem here.
		// Cannot read property 'instancedArrays' of undefined. But on another project with a similar
		// setup, I get an error that I need to import StandardMaterial. This happens whether or not
		// I provide a real URL for the dome photo.
		const sky = new PhotoDome("photoDome", "", { size: 1000 }, scene);

		engine.runRenderLoop(function () {
			scene.render();
		});
	},
});
</script>
