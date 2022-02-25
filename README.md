# Shader Material
```.tsx
const ShaderPlane: VFC = () => {
	const shader: THREE.Shader = {
		uniforms: {
			u_time: { value: 0 }
		},
		vertexShader: vertexShader,
		fragmentShader: fragmentShader
	}

	useFrame(() => {
		shader.uniforms.u_time.value += 0.005
	})

	return (
		<Plane args={[2, 2]}>
			<shaderMaterial args={[shader]} />
		</Plane>
	)
}

const vertexShader = `
varying vec2 v_uv;

void main() {
  v_uv = uv;
  gl_Position = projectionMatrix * modelViewMatrix * vec4( position, 1.0 );
}
`

const fragmentShader = `
uniform float u_time;
varying vec2 v_uv;

void main() {
  vec3 color = vec3(v_uv, 1.0);
  gl_FragColor = vec4(color, 1.0);
}
`
```

# Post-processing Pass
```.tsx
const datas = {
	enabled: true,
}

export const CustomPass: VFC = () => {
	const passRef = useRef<ShaderPass>(null)

	// const gui = GUIController.instance.setFolder('CustomPass')
	// gui.addCheckBox(datas, 'enabled')

	const shader: THREE.Shader = {
		uniforms: {
			tDiffuse: { value: null }
		},
		vertexShader: vertexShader,
		fragmentShader: fragmentShader
	}

	const update = () => {
		passRef.current!.enabled = datas.enabled

		if (datas.enabled) {
		}
	}

	useFrame(() => {
		update()
	})

	return <shaderPass ref={passRef} attachArray="passes" args={[shader]} />
}

const vertexShader = `
varying vec2 v_uv;

void main() {
  v_uv = uv;

  gl_Position = projectionMatrix * modelViewMatrix * vec4( position, 1.0 );
}
`

const fragmentShader = `
uniform sampler2D tDiffuse;
varying vec2 v_uv;

void main() {
  vec4 tex = texture2D(tDiffuse, v_uv);
  
  gl_FragColor = tex;
}
`
```
