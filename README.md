# OAK-D Modular Component

This is a [Viam module](https://docs.viam.com/manage/configuration/#modules) for the [OAK-D](https://shop.luxonis.com/products/oak-d) camera. Registered at https://app.viam.com/module/viam/oak-d.

## Getting Started

### Checking Python version

First and foremost, open a terminal on your robot, and run the following command to check its Python version:

```console
$ python3 --version
```

Verify that your robot's Python3 version is 3.8 or later, and that it is installed and linked to the `python3` command to avoid compatibility issues.

### Using the registry

The recommended way to install the module is through the Viam registry.

- Go to your robot's page on app.viam.com.
- Click on the *Create Component* button in the Components section.
- Search for the *oak-d* module and select it. 

This will automatically install the module to your robot.


### Locally installing the module

If you do not want to use the Viam registry, you can use the module from source [here](https://github.com/viamrobotics/viam-camera-oak-d).

```console
$ cd <your-directory>
$ git clone https://github.com/viamrobotics/viam-camera-oak-d.git
```

Then modify your robot's JSON file as follows

```
  "modules": [
    {
      "type": "local",
      "name": "oak-d",
      "executable_path": "<your-directory>/viam-camera-oak-d/run.sh"
    }
  ],
```

## Attributes and Sample Config

The attributes for the module are as follows:
- `sensors` (required): a list that contain the strings `color` and/or `depth`. The sensor that comes first in the list is designated the "main sensor" and will be the image that gets returned by `get_image` calls and what will appear in the Control tab on app.viam. When both sensors are requested, `get_images` will return both the color and depth outputs captured at the same timestamp, and `get_point_clouds` will also be available for use. See the [camera docs](https://docs.viam.com/components/camera/#api) for more details on these functions. 
- `width_px`, `height_px`: the int width and height of the output images. If the OAK-D cannot produce the requested resolution, the component will be configured to the closest resolution to the given height/width. Therefore, the image output size will not always match the input size. Color and depth outputs will always output with the same height and width.
- `frame_rate`: the float that represents the frame rate the camera will capture images at.
- `debug`: the bool that determines whether the module will log debug messages in `std.out`. Note: make sure you also use the `-debug` flag 
when starting the Viam server in order for debug logs to be piped properly e.g. `viam-server -debug -config /etc/viam.json`.
```
{
  "components": [
    {
      "name": "my-oak-d-camera",
      "attributes": {
        "sensors": ["color", "depth"],
        "width_px": 640,
        "height_px": 480,
        "frame_rate": 30,
        "debug": false
      },
      "namespace": "rdk",
      "type": "camera",
      "model": "viam:camera:oak-d"
    }
  ]
}
```

## Integration Tests

The repo comes with a suite of integration tests that allow one to test if the module works with an actual OAK-D device on the machine of interest. You will need to compile the binary on the same machine you expect to run it on.

- Copy the repo to your local robot: `git clone https://github.com/viamrobotics/viam-camera-oak-d.git`
- Run `make integration-tests`
