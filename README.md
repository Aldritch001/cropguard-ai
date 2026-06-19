# CropGuard AI

An AI powered crop disease detector for smallholder farmers, built around a model that diagnoses plant diseases from a single photo.

The end goal is a WhatsApp bot. A farmer sends a photo of a sick looking leaf, and gets back a diagnosis with treatment advice in plain language. This repo documents the build from raw data through to a trained model, with the bot itself coming in a later stage.

## Status
Model trained and evaluated. Working on the WhatsApp integration next.

## Built by
Absolon

## The model

Trained using transfer learning on EfficientNetB0, pretrained on ImageNet, with a custom classification head fine tuned on the PlantVillage dataset. The base network's weights were frozen, only the new classification layers were trained, which keeps training fast and works well with a dataset of this size.

23 classes covering Corn, Grape, Pepper, Potato and Tomato, spanning both diseased and healthy leaf states.

**Test set results**
- Accuracy: 94.77%
- Loss: 0.1675
- Evaluated on 4,625 held out test images never seen during training

**Where it's strongest**
Corn healthy, Grape Black Rot, Grape Esca, Pepper bell healthy, and Tomato Yellow Leaf Curl Virus all score at or near perfect precision and recall. These tend to be visually distinct conditions.

**Where it struggles**
Corn Cercospora leaf spot has the lowest recall (0.76), most often confused with Corn Northern Leaf Blight, two diseases that genuinely look similar on a leaf, especially depending on how advanced the infection is. Tomato Target Spot has the lowest precision (0.81), occasionally confused with Tomato Late Blight and Tomato Bacterial Spot for the same reason. Worth being upfront about this rather than just quoting the headline accuracy number, since it's exactly the kind of thing that matters for a real diagnostic tool.

## Tech stack
- TensorFlow / Keras
- EfficientNetB0 (transfer learning)
- Google Colab (free GPU training)
- PlantVillage dataset (Kaggle)

## Roadmap
- [x] Data exploration and train / validation / test split
- [x] Model training and evaluation
- [ ] WhatsApp bot integration (Flask API + Twilio)
- [ ] Deployment
- [ ] Pilot with real farmers
