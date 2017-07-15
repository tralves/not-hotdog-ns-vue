<template>
    <stack-layout>
        <Label class="h2 m-t-5" text='Step 1: choose a picture'/>
        <stack-layout orientation="horizontal">
            <Button class="btn btn-primary btn-rounded-lg" ref="imageFromCamera" @tap="imageFromCamera">Camera</Button>
            <Button class="btn btn-primary btn-rounded-lg" ref="imageFromGallery" @tap="imageFromGallery">Gallery</Button>
        </stack-layout>
        <Image :src="picture" class="m-y-5" height="210"/>
        <Label class="h2 m-t-5" text='Step 2: Is it a hotdog?' ref="hotdogOrNotLabel" visibility="collapse"/>
        <Button class="btn btn-primary btn-rounded-lg" ref="hotdogOrNotButton" @tap="isHotdogOrNot" visibility="collapse">Hotdog or Not?</Button>
    </stack-layout>
</template>

<script>
import * as camera from "nativescript-camera";
import * as ImageSource from "image-source";
import * as ImagePicker from "nativescript-imagepicker";
import * as Dialogs from "ui/dialogs";

const http = require("http");


export default {
    data: () => {
        return {
            val: 'Vue.js',
            picture: null
        }
    },
    methods: {
        imageFromCamera () {
            console.log("Picking Image!");
            console.log(camera);
            camera.requestPermissions();

            var options = { width: 200, height: 200, keepAspectRatio: true, saveToGallery: true };
            camera.takePicture(options)
                .then( (imageAsset) => {
                    console.log("Size: " + imageAsset.options.width + "x" + imageAsset.options.height);
                    console.log("keepAspectRatio: " + imageAsset.options.keepAspectRatio);
                    console.log("Photo saved in Photos/Gallery for Android or in Camera Roll for iOS");
                    this.picture = imageAsset;
                    this.$refs.hotdogOrNotButton.nativeView.visibility = "visible";
                    this.$refs.hotdogOrNotLabel.nativeView.visibility = "visible";
                }).catch( (err) => {
                    console.log("Error -> " + err.message);
                });
        },
        imageFromGallery () {
            var context = ImagePicker.create({
                mode: "single"
            });

            context
                .authorize()
                .then( () => {
                    return context.present();
                })
                .then( selection => {
                    console.log("Selection done:");
                    selection.forEach( (selected) => {
                        console.log(" - " + selected.uri);
                        this.picture = selected;
                        this.$refs.hotdogOrNotButton.nativeView.visibility = "visible";
                        this.$refs.hotdogOrNotLabel.nativeView.visibility = "visible";
                    });
                }).catch( (e) => {
                    console.log(e);
                });
        },
        isHotdogOrNot () {

            ImageSource.fromAsset(this.picture)
                .then( imgSrc => {
                    const imageAsBase64 = imgSrc.toBase64String("jpg");
                    console.log('picture b ' + (typeof imageAsBase64));
                    console.log('making request ' + imageAsBase64.length);
                    try {


                        http.request({
                            url: "https://api.clarifai.com/v2/models/aaa03c23b3724a16a56b629203edc62c/outputs",
                            method: "POST",
                            headers: {
                                "Content-Type": "application/json",
                                "Authorization": "Key e65d726322664ad1b046fb5ee074114c",
                            },
                            content: JSON.stringify({
                                "inputs": [{
                                    "data": {
                                        "image": {
                                            "base64": imageAsBase64 //"https://samples.clarifai.com/metro-north.jpg"
                                        }
                                    }
                                }]
                            })
                        }).then( response => {
                            console.log('response!!!!');
                            console.log(response.content.toString());
                            try {
                                var result = response.content.toJSON();

                                console.dir(result);

                                var things = result.outputs[0].data.concepts.map( mc => mc.name );
                                console.log(things);
                                if (things.indexOf('hotdog') >= 0) {
                                    Dialogs.alert('Hotdog!!')
                                }
                                else {
                                    Dialogs.alert('No hotdog... :(');
                                }
                            }
                            catch (e) {
                                console.log('error parsing response: ' + e);
                            }
                            console.dir(result);
                        }, e => {
                            console.log("Error occurred " + e);
                        });

                    }
                    catch (e) {
                        console.log('error in request: ' + e);
                    }

                });

        }
    }
}





</script>