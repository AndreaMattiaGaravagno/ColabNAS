# ColabNAS
A hardware-aware neural architecture search (HW NAS) algorithm targeting low-RAM microcontrollers. It provides CNNs that run on low-RAM microcontrollers, e.g. 40 kiB, using online GPU programs like Google Colaboratory or Kaggle Kernel.

If you want a faster and similar HW NAS at the expense of being less repeatable and precise, check [NanoNAS](https://github.com/AndreaMattiaGaravagno/NanoNAS).

# News
* **2023/11** ColabNAS has been accepted for publication in the [Future Generation Computer Systems journal](https://www.sciencedirect.com/science/article/pii/S0167739X23004028)! Experimental Data is available at the following [link](https://drive.google.com/drive/folders/14wkOeM7TcNkZLpWwrVJRjHrxt0LG_7Ad?usp=sharing) in the form of Jupyter Notebook.
* **2023/09** ColabNAS has been cited in a tinyML talk on [YouTube](https://www.youtube.com/watch?v=syY4mwSeC6Q).

# An overview of its performances
This section evaluates the hardware-aware feature of ColabNAS by providing results for different hardware targets. To highlight the ColabNAS feature of providing lightweight CNNs, three low-RAM STMicroelectronics (STM) MCUs from the Ultra-low Power series have been selected: the L010RBT6, the L151UCY6DTR and the L412KBU3.

| STM32 MCU   | RAM    | Flash   | CoreMark |
| :---        | :---:  | :---:   | :---:    |
| L010RBT6    | 20 kiB | 128 kiB | 75       | 
| L151UCY6DTR | 32 kiB | 256 kiB | 93       |
| L412KBU3    | 40 kiB | 128 kiB | 273      |

The values in the previous table set the constraints for running ColabNAS on each target. RAM and Flash were provided as is, while the MACC upper bound was obtained by multiplying by $10^{4}$ the CoreMark score of the target, in order to allow a fair exploration of the search space. 50x50x3 input size has been used to cope with MCUs' constrained resources. The results for the [Visual Wake Words dataset](https://arxiv.org/abs/1906.05721), a [standard TinyML benchmark](https://arxiv.org/abs/2003.04821), follow.

| Target      | TFlite Model | Accuracy | RAM      | Flash    | MACC    | Search Cost |
| :---        | :---:        | :---:    | :---:    | :---:    | :---:   | :---:       |
| L010RBT6    | [link](https://drive.google.com/file/d/103XvW4AlDvXwXaIPEtcK3RtAjTlY7Qif/view?usp=drive_link) | 69.4%    | 19 kiB   | 8.02 kiB | 227 k   | 2h:11m      | 
| L151UCY6DTR | [link](https://drive.google.com/file/d/1-iR347s0TgHGNbCbt3UQBXZ9aulN0st0/view?usp=drive_link) | 74.5%    | 22.5 kiB | 18.5 kiB | 657 k   | 3h:04m      |
| L412KBU3    | [link](https://drive.google.com/file/d/10f_9eumbIQwTHg0HQJBNdkfezcvpU15y/view?usp=drive_link) | 77.8%    | 33 kiB   | 44.9 kiB | 2,086 k | 2h:47m      |

For more experimental results refer to the following paper, which you can find [here](https://arxiv.org/abs/2212.07700), and to [this Google Colaboratory notebook](https://colab.research.google.com/drive/1VeTPJxo_klFdav727n-YXkjOQUcjTj9w?usp=drive_link). 

# How to use

* Upload the stm32tflm script in the files folder of Google Colaboratory's VM
* In the cell "example of usage" of the Jupyter Notebook modify: 
  * "path_to_training_set" and "path_to_test_set" variables to use your own dataset ([a small-dataset suggestion](https://www.kaggle.com/datasets/hasnainjaved/melanoma-skin-cancer-dataset-of-10000-images?resource=download))
  * "input_size" to set the network's input size (50x50 is a good starting point for microcontrollers)
  * "RAM_upper_bound" and "Flash_upper_bound" according to your microcontroller
  * "MACC_upper_bound" according to the maximum desired number of MACC (a good starting point is given by multiplying the [CoreMark](https://www.eembc.org/coremark/) score of your microcontroller by 10000)
* Run the cell

In the folder "results", you will find a copy of the trained Keras model and the corresponding fully quantized Tflite model at uint8, ready to run on a microcontroller.

# Citation
If you find the project helpful, please consider citing our paper:

    @misc{garavagno2022colab,
        title={Colab NAS: Obtaining lightweight task-specific convolutional neural networks following Occam's razor}, 
        author={Andrea Mattia Garavagno and Daniele Leonardis and Antonio Frisoli},
        year={2022},
        eprint={2212.07700},
        archivePrefix={arXiv},
        primaryClass={cs.CV}
    }
