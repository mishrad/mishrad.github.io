---
title: "E-ink Picture generation"
date: 2026-08-04
summary: "Create a small browser app which generates images"
---

# E-ink Picture generation

I wanted to create a browser app that generates images. My toddler is learning new words and I was trying to work with creating an image based on the words she speaks. The current fav - a turtle, a moon, and a fish. Each picture is new. A small computer at home generates them.

## How it works

My old Kindle has a basic browser and nothing else. It loads one web page from a mini PC on our network. The page has one image. Tap it and you get the next one.

The mini PC runs a Flask server and [SD-Turbo](https://huggingface.co/stabilityai/sd-turbo), an image model that works on a CPU. Each request picks a word from a text file and puts it into a fixed prompt. The prompt asks for simple grayscale drawings on a white background for a children's story book which is useful as the old kindle I have cannot render color. 

To change the pictures, I edit the text file. Add "rocket" on a new line and rockets start showing up.

## Current list of bugs

The first image worked. The second crashed. The model pipeline stays in memory between requests, and a library component kept a counter from the previous run. Fresh state on every call fixed it.

Then a crash from double taps. Generation took a minute, the screen showed nothing in the meantime, so a second tap arrived. Two requests hit the same model at once and one died. Instead of adding a lock, I moved generation to a background worker that keeps a stock of finished images in a queue. A tap takes one from the queue. The wait went from a minute to nothing.

Then the picture stopped changing. The Kindle browser caches by URL and would not refetch the page. I added a timestamp to every link and no-store headers to every response.

## Speed

PyTorch on CPU took about a minute per image. I converted the model to [OpenVINO](https://github.com/openvinotoolkit/openvino), which compiles it for Intel chips. Now it takes a few seconds. The queue hides even that. Tap, a picture appears, and the worker refills the shelf in the background.

## Code

It is a basic project but was a lot of fun. I plan to keep adding to it. The current version is on [GitHub](https://github.com/mishrad/homeprojects/tree/main/eink-art).

The prompt is fixed and the word list is the only input. Turtles and moons in, turtles and moons out. 

## Caution
Image generation is a tough problem and requires lot of computing. I am still trying to understand how to make it safer and the images a bit better to look at. It was still fun to work on. 
