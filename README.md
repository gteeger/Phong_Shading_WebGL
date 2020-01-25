

<script id="vertex-shader" type="x-shader/x-vertex">
    attribute  vec4 vPosition;
    attribute  vec3 vNormal;
    varying vec4 fColor;
    uniform vec4 ambientProduct, diffuseProduct, specularProduct;
    uniform mat4 modelViewMatrix;
    uniform mat4 modelViewMatrix_L;

    uniform mat4 projectionMatrix;
    //uniform vec4 lightPosition;
    uniform float shininess;
    uniform int is_light;
    //uniform mat3 normalMatrix;
    void main()
    {

    vec4 lightPosition;
    lightPosition = vec4(0.0, 0.0, 0.0, 1.0);

    float limit =  0.5236;

    mat4 lightMat = mat4(1.0);

    lightPosition =  modelViewMatrix  * modelViewMatrix_L *  lightPosition;
    vec3 pos = -(modelViewMatrix * vPosition).xyz;

    //fixed light postion

    vec3 light = lightPosition.xyz;
    vec3 L = normalize( light - pos );

    vec3 E = normalize( -pos );
    vec3 H = normalize( L + E );

    vec4 NN = vec4(vNormal,0);
    // Transform vertex normal into eye coordinates

    vec3 N = normalize( (modelViewMatrix  * modelViewMatrix_L * NN).xyz);


    // Compute terms in the illumination equation
    vec4 ambient = ambientProduct;

    float Kd = max( dot(L, N), 0.0 );
    vec4  diffuse = Kd*diffuseProduct;

    float Ks = pow( max(dot(N, H), 0.0), shininess );
    vec4  specular = Ks * specularProduct;

    //float dotFromDirection = dot(surfaceToLightDirection,-u_lightDirection);
    //float dotFromDirection = dot(N, vec4(0.0, 0.0, 0.0, 0.0));

    if( dot(L, N) < 0.0  ) {
    specular = vec4(0.0, 0.0, 0.0, 1.0);
    }

    gl_Position = projectionMatrix * modelViewMatrix * vPosition;

    if(is_light==1){
    fColor = vec4(1.0, 1.0, 1.0, 1.0);
    }
    else{
    fColor = ambient + diffuse + specular;

    //fColor = ambient + specular;
    fColor.a = 1.0;
    }


    }
</script>

    <script id="fragment-shader" type="x-shader/x-fragment">
        precision mediump float;
        varying vec4 fColor;
        void
        main()
        {
        gl_FragColor = fColor;
        }
    </script>
    <script type="text/javascript" src="Common/webgl-utils.js"></script>
    <script type="text/javascript" src="Common/initShaders.js"></script>
    <script type="text/javascript" src="Common/MV.js"></script>
    <script type="text/javascript" src="js/main.js"></script>
    <script type="text/javascript" src="js/bunny.js"></script>
	<link href="style.css" rel="stylesheet">


	
    <div class="container">
        <body>
			<h2>This is a 3D model of a bunny with the Blinn-Phong model used for lighting</h1>
			<h3>The Phong shading model is the sum of three lighting components: ambient, diffuse, and specular.
				You can adjust the parameters of these 3 components with the sliders below. 
<ul>
				<li>Left click on the canvas and drag for X,Y translation.</li>
				<li>Press Up and down arrows for Z translation</li>
				<li>Right click and drag for X,Y rotation</li>
				<li>Press R to reset position</li>
				<li>Press P to stop the rotation of the light </li>
				<li>The rotating cube is a point light source </li>	
</ul>			
			</h3>
			<br />

            <canvas id="gl-canvas" width="700" " height="500">
                Oops ... your browser doesn't support the HTML5 canvas element
            </canvas>
        </body>
        <br />
    </div>
<div class="row">
<div class="column">
<b>Perspective settings:</b> These sliders specify the shape of the field of view (Research "Frustum")
        <div>
            zNear .01<input id="zNearSlider" type="range"
                            min="0.01" max="10.0" step="0.1" value="0.3" />
            10
        </div>
        <div>
            zFar 3<input id="zFarSlider" type="range"
                         min="3" max="20.0" step="0.1" value="10.0" />
            20
        </div>

        <div>
            field of view 10<input id="fovSlider" type="range"
                         min="10" max="120" step="5" value="45" />
            120
        </div>
        <div>
            aspect ratio 0.5<input id="aspectSlider" type="range"
                             min="0.5" max="2" step="0.1" value="1" />
            2
        </div>
</div>
<div class="column">

<b>Camera Position:</b>
        <div>
            x 0.0<input id="eyex" type="range"
                           min="0.0" max="25.0" step="0.5" value="0.0" />
            100.0
<br>
            y 0.0<input id="eyey" type="range"
                           min="0.0" max="100.0" step="0.5" value="5.0" />
            100.0
<br>
            z 0.0<input id="eyez" type="range"
                           min="-0.0" max="100.0" step="0.5" value="5.0" />
            100.0
        </div>
</div>
</div>
        --------------------------------------------------------------------------
<br>
<b>Phong Lighting Settings:</b>
<br>
<div class="row">
<div class="column">
<u>Light Settings:</u>
        <div>
            Red Ambient -5.0<input id="la1" type="range"
                           min="-5.0" max="5.0" step="0.01" value="0.2" />
            5.0
<br>
            Green Ambient -5.0<input id="la2" type="range"
                           min="-5.0" max="5.0" step="0.01" value="0.2" />
            5.0
<br>
            Blue Ambient -5.0<input id="la3" type="range"
                           min="-5.0" max="5.0" step="0.01" value="0.2" />
            5.0

        </div>

<br>

        <div>
            Red Diffuse -5.0<input id="ld1" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0
<br>
            Blue Diffuse -5.0<input id="ld2" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0
<br>
            Green Diffuse -5.0<input id="ld3" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0

        </div>

<br>
        <div>
            Red Specular -5.0<input id="ls1" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0
<br>
            Green Specular -5.0<input id="ls2" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0
<br>
            Blue Specular -5.0<input id="ls3" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0

        </div>
        <div>
            shine 0.0<input id="shine" type="range"
                            min="0" max="1000.0" step="0.1" value="1.0" />
            1000.0
        </div>

</div>
<div class="column">
<u>Material Settings:</u>

        <div>
            Red Ambient -5.0<input id="ma1" type="range"
                           min="-5.0" max="5.0" step="0.01" value="0.2" />
            5.0
<br>
            Green Ambient -5.0<input id="ma2" type="range"
                           min="-5.0" max="5.0" step="0.01" value="0.2" />
            5.0
<br>
            Blue Ambient -5.0<input id="ma3" type="range"
                           min="-5.0" max="5.0" step="0.01" value="0.2" />
            5.0


        </div>

<br>

        <div>
            Red Diffuse -5.0<input id="md1" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0
<br>
            Green Diffuse -5.0<input id="md2" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0
<br>
            Blue Diffuse -5.0<input id="md3" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0


        </div>
<br>

        <div>
            Red Specular -5.0<input id="ms1" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" /> 5.0
<br>
            Green Specular -5.0<input id="ms2" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0
<br>
            Blue Specular -5.0<input id="ms3" type="range"
                           min="-5.0" max="5.0" step="0.01" value="1.0" />
            5.0


        </div>
<br>


</div>
</div>
--------------------------------------------------------------------------

