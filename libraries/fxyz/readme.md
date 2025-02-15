## How to build

The project is managed by gradle. To build, type

```shell
./gradlew assemble
```

To deploy it to your local Maven repository, type:

```shell
./gradlew publishToMavenLocal
```

## Use of FXyz3D Core

With FXyz3D there are many different 3D custom shapes. See for example `SpringMesh` in the screenshots, to create
a 3D mesh of a spring.

### Sample

Create a gradle project, edit the build.gradle file and add:

```
plugins {
    id 'application'
    id 'org.openjfx.javafxplugin' version '0.0.10'
}

mainClassName = 'org.fxyz3d.Sample'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.fxyz3d:fxyz3d:0.5.4'
}

javafx {
    modules = [ 'javafx.controls' ]
}
```

and create a JavaFX Application class `Sample` under the `org.fxyz3d` package:

```java
    @Override
    public void start(Stage primaryStage) throws Exception {
        PerspectiveCamera camera = new PerspectiveCamera(true);        
        camera.setNearClip(0.1);
        camera.setFarClip(10000.0);
        camera.setTranslateX(10);
        camera.setTranslateZ(-100);
        camera.setFieldOfView(20);
        
        CameraTransformer cameraTransform = new CameraTransformer();
        cameraTransform.getChildren().add(camera);
        cameraTransform.ry.setAngle(-30.0);
        cameraTransform.rx.setAngle(-15.0);
        
        SpringMesh spring = new SpringMesh(10, 2, 2, 8 * 2 * Math.PI, 200, 100, 0, 0);
        spring.setCullFace(CullFace.NONE);
        spring.setTextureModeVertices3D(1530, p -> p.f);
        
        Group group = new Group(cameraTransform, spring);
        
        Scene scene = new Scene(group, 600, 400, true, SceneAntialiasing.BALANCED);
        scene.setFill(Color.BISQUE);
        scene.setCamera(camera);
        
        primaryStage.setScene(scene);
        primaryStage.setTitle("FXyz3D Sample");
        primaryStage.show();
    }
```

### FXSampler

To use the FXSampler and visualize all the samples and the different options available, run:

```shell
./gradlew run
```

There is a hidden side popup menu at the left, from where different samples can be selected. From the right panels different options can be applied dynamically to the 3D shape (see screenshots).

#### Custom image

You can create a custom image for your platform running:

```shell
./gradlew clean :FXyz-Samples:jlink  
```

And you can run it with Java 9+ on your platform:

```shell
FXyz-Samples/build/FXyz/bin/FXyzSamples
```