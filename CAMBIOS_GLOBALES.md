# Cambio de globales a locales en `MAIN_fixed.txt`

## Resumen

- **325 globales** pasan a ser variables locales, en los 8 scripts que tenían locales libres.
- **Globales distintas:** pasan de 1086 a 761.
- **Global numerada más alta:** pasa de `$2100` a `$1454`. Como la partida guarda todas las globales desde `$0` hasta la más alta, este es el cambio que más reduce el bloque de globales del guardado.
- **Globales con nombre:** pasan de 201 a 28. Desaparecen, por ejemplo, las 170 `$genNNNgen` de INITIAL.
- **Opcodes cambiados:** 90 líneas. Solo se cambió cuando la línea usaba un opcode exclusivo de **globales** y la variable pasó a ser **local**. El detalle está más abajo.
- **Comentarios:** no se cambió ninguno. Algunas líneas conservan comentarios del descompilador como `// $ = $ int`, aunque ahora tengan una local.

## Reglas usadas

1. **Un solo script.** Solo se convierte una global si todas las líneas que la usan las ejecuta un único hilo o misión, contando los `gosub` y los saltos que llegan a esas líneas.
2. **Locales libres.** Solo se usan índices (`0@`–`15@`) que ese script no usa en ninguna parte. Nunca se tocan `16@`/`17@` (TIMERA/TIMERB).
3. **Compartir un mismo índice.** Varias globales comparten índice local solo si nunca necesitan su valor al mismo tiempo (análisis de vida de variables).
4. **Globales que arrastran su valor.** Las que se leen antes de escribirse, como los manejadores de zombis guardados de una vuelta a la siguiente, solo se convirtieron en hilos que se lanzan una vez y nunca se matan: `EAT`, `MEAT`, `BEAT` y `PSAVE1`. Las locales de un hilo también se guardan en la partida, así que conservan el valor igual que antes.
5. **Opcodes que solo admiten globales.** No se tocaron las globales que usan los contadores y temporizadores en pantalla (`03C4`, `03C3`, `04F7`, `014F`, `0151`) ni `$onmission` (`0180`).
6. **Comprobación final.** Se verificó automáticamente que, en cada script modificado, cada lectura de variable sigue viendo exactamente las mismas escrituras que en el original (1317 lecturas, 0 diferencias). Las etiquetas, saltos y comentarios no cambiaron.

## Globales que se quedaron como globales a propósito

| Global | Script | Motivo |
|---|---|---|
| `$214` | Hilo EAT (zombis de delante) | No quedan locales libres en EAT (las 10 libres ya las usan `$204`–`$213`) |
| `$735` | Hilo DRIVE | Es una bandera que conserva su valor entre ejecuciones, y `DRIVE` se mata y se relanza desde la misión 3; como local se reiniciaría a 0 |

## Opcodes cambiados

Siempre es el mismo opcode en su versión para locales; la operación es idéntica:

| Antes | Después | Veces |
|---|---|---|
| `0004` SET_VAR_INT | `0006` SET_LVAR_INT | 6 |
| `0008` ADD_VAL_TO_INT_VAR | `000A` ADD_VAL_TO_INT_LVAR | 3 |
| `0018` IS_INT_VAR_GREATER_THAN_NUMBER | `0019` IS_INT_LVAR_GREATER_THAN_NUMBER | 3 |
| `0038` IS_INT_VAR_EQUAL_TO_NUMBER | `0039` IS_INT_LVAR_EQUAL_TO_NUMBER | 40 |
| `0084` SET_VAR_INT_TO_VAR_INT | `008A` SET_VAR_INT_TO_LVAR_INT | 19 |
| `0084` SET_VAR_INT_TO_VAR_INT | `008B` SET_LVAR_INT_TO_VAR_INT | 19 |

Líneas: 318, 405, 407, 413, 500, 502, 509, 587, 589, 732, 734, 738, 740, 744, 746, 750, 752, 756, 758, 762, 764, 768, 770, 774, 776, 780, 782, 786, 795, 797, 801, 803, 808, 810, 814, 816, 820, 822, 826, 828, 832, 834, 838, 840, 844, 846, 850, 860, 862, 866, 868, 872, 874, 878, 880, 884, 886, 890, 892, 900, 902, 906, 908, 912, 914, 918, 920, 924, 926, 930, 932, 940, 942, 946, 948, 952, 954, 958, 960, 968, 970, 974, 976, 980, 982, 986, 988, 5226, 5236, 5827

## Tabla de cambios por script

### Misión 0 — INITIAL

Locales libres antes del cambio: 0@, 1@, 2@, 3@, 4@, 5@, 6@, 7@, 8@, 9@, 10@, 11@, 12@, 13@, 14@, 15@.

| Global | Ahora | Líneas |
|---|---|---|
| `$15` | `0@` | 5236 |
| `$333` | `0@` | 5276, 5277, 5284 |
| `$334` | `0@` | 5289, 5290 |
| `$335` | `0@` | 5295, 5296 |
| `$336` | `0@` | 5297, 5298 |
| `$337` | `0@` | 5299, 5300 |
| `$338` | `0@` | 5301, 5302 |
| `$339` | `0@` | 5303, 5304 |
| `$341` | `0@` | 5308, 5309 |
| `$342` | `0@` | 5310, 5311 |
| `$343` | `0@` | 5312, 5313 |
| `$344` | `0@` | 5314, 5315 |
| `$345` | `0@` | 5316, 5317 |
| `$346` | `0@` | 5318, 5319 |
| `$347` | `0@` | 5320, 5321, 5322 |
| `$348` | `0@` | 5323, 5324 |
| `$349` | `0@` | 5325, 5326 |
| `$350` | `0@` | 5327, 5328 |
| `$351` | `0@` | 5329, 5330 |
| `$354` | `0@` | 5339, 5340 |
| `$355` | `0@` | 5341, 5342 |
| `$356` | `0@` | 5343, 5344 |
| `$357` | `0@` | 5345, 5346 |
| `$358` | `0@` | 5347, 5348 |
| `$359` | `0@` | 5349, 5350 |
| `$361` | `0@` | 5354, 5355 |
| `$362` | `0@` | 5356, 5357, 5358 |
| `$363` | `0@` | 5359, 5360, 5361, 5362, 5363 |
| `$364` | `0@` | 5364, 5365, 5366, 5367, 5368 |
| `$365` | `0@` | 5369, 5370, 5371, 5372, 5373 |
| `$366` | `0@` | 5374, 5375, 5376, 5377, 5378 |
| `$367` | `0@` | 5379, 5380, 5381, 5382, 5383 |
| `$368` | `0@` | 5384, 5385, 5386, 5387, 5388 |
| `$369` | `0@` | 5389, 5390 |
| `$370` | `0@` | 5391, 5392 |
| `$542` | `0@` | 5736 |
| `$543` | `0@` | 5737 |
| `$544` | `0@` | 5738 |
| `$545` | `0@` | 5739 |
| `$546` | `0@` | 5740 |
| `$547` | `0@` | 5741 |
| `$549` | `0@` | 5743 |
| `$551` | `0@` | 5745 |
| `$552` | `0@` | 5746 |
| `$553` | `0@` | 5747 |
| `$554` | `0@` | 5748 |
| `$555` | `0@` | 5749 |
| `$556` | `0@` | 5750 |
| `$557` | `0@` | 5751 |
| `$558` | `0@` | 5752 |
| `$559` | `0@` | 5753 |
| `$561` | `0@` | 5755 |
| `$562` | `0@` | 5756 |
| `$563` | `0@` | 5757 |
| `$564` | `0@` | 5758 |
| `$565` | `0@` | 5759 |
| `$566` | `0@` | 5760 |
| `$567` | `0@` | 5761 |
| `$568` | `0@` | 5762 |
| `$569` | `0@` | 5763 |
| `$570` | `0@` | 5764 |
| `$571` | `0@` | 5765 |
| `$572` | `0@` | 5766 |
| `$573` | `0@` | 5767 |
| `$574` | `0@` | 5768 |
| `$575` | `0@` | 5769 |
| `$576` | `0@` | 5770 |
| `$577` | `0@` | 5771 |
| `$578` | `0@` | 5772 |
| `$579` | `0@` | 5773 |
| `$580` | `0@` | 5774 |
| `$581` | `0@` | 5775 |
| `$582` | `0@` | 5776 |
| `$583` | `0@` | 5777 |
| `$584` | `0@` | 5778 |
| `$585` | `0@` | 5779 |
| `$586` | `0@` | 5780 |
| `$587` | `0@` | 5781 |
| `$588` | `0@` | 5782 |
| `$589` | `0@` | 5783 |
| `$590` | `0@` | 5784 |
| `$591` | `0@` | 5785 |
| `$592` | `0@` | 5786 |
| `$593` | `0@` | 5787 |
| `$594` | `0@` | 5788 |
| `$595` | `0@` | 5789 |
| `$596` | `0@` | 5790 |
| `$597` | `0@` | 5791 |
| `$598` | `0@` | 5792 |
| `$600` | `0@` | 5794 |
| `$601` | `0@` | 5795 |
| `$602` | `0@` | 5796 |
| `$603` | `0@` | 5797 |
| `$604` | `0@` | 5798 |
| `$605` | `0@` | 5799 |
| `$606` | `0@` | 5800 |
| `$607` | `0@` | 5801 |
| `$608` | `0@` | 5802 |
| `$609` | `0@` | 5803 |
| `$610` | `0@` | 5804 |
| `$611` | `0@` | 5805 |
| `$612` | `0@` | 5806 |
| `$613` | `0@` | 5807 |
| `$614` | `0@` | 5808 |
| `$615` | `0@` | 5809 |
| `$616` | `0@` | 5810 |
| `$617` | `0@` | 5811 |
| `$618` | `0@` | 5812 |
| `$619` | `0@` | 5813 |
| `$620` | `0@` | 5814 |
| `$622` | `0@` | 5816 |
| `$623` | `0@` | 5817 |
| `$624` | `0@` | 5818 |
| `$625` | `0@` | 5819 |
| `$626` | `0@` | 5820 |
| `$627` | `0@` | 5821 |
| `$628` | `0@` | 5822 |
| `$629` | `0@` | 5823 |
| `$630` | `0@` | 5824 |
| `$631` | `0@` | 5825 |
| `$632` | `0@` | 5826 |
| `$633` | `0@` | 5827 |
| `$arena_door_1` | `0@` | 5285, 5286 |
| `$arena_door_2` | `0@` | 5287, 5288 |
| `$default_wait_time` | `0@` | 5226 |
| `$gen371gen` | `0@` | 5394, 5395 |
| `$gen372gen` | `0@` | 5396, 5397 |
| `$gen373gen` | `0@` | 5398, 5399 |
| `$gen374gen` | `0@` | 5400, 5401 |
| `$gen375gen` | `0@` | 5402, 5403 |
| `$gen376gen` | `0@` | 5404, 5405 |
| `$gen377gen` | `0@` | 5406, 5407 |
| `$gen378gen` | `0@` | 5408, 5409, 5424, 5425 |
| `$gen379gen` | `0@` | 5410, 5411 |
| `$gen380gen` | `0@` | 5412, 5413 |
| `$gen381gen` | `0@` | 5414, 5415 |
| `$gen382gen` | `0@` | 5416, 5417 |
| `$gen383gen` | `0@` | 5418, 5419 |
| `$gen384gen` | `0@` | 5420, 5421 |
| `$gen385gen` | `0@` | 5422, 5423 |
| `$gen386gen` | `0@` | 5426, 5427 |
| `$gen387gen` | `0@` | 5428, 5429 |
| `$gen388gen` | `0@` | 5430, 5431 |
| `$gen389gen` | `0@` | 5432, 5433 |
| `$gen390gen` | `0@` | 5434, 5435 |
| `$gen391gen` | `0@` | 5436, 5437 |
| `$gen392gen` | `0@` | 5438, 5439 |
| `$gen393gen` | `0@` | 5440, 5441 |
| `$gen395gen` | `0@` | 5442, 5443 |
| `$gen396gen` | `0@` | 5444, 5445 |
| `$gen397gen` | `0@` | 5446, 5447 |
| `$gen398gen` | `0@` | 5448, 5449 |
| `$gen399gen` | `0@` | 5450, 5451 |
| `$gen400gen` | `0@` | 5452, 5453 |
| `$gen401gen` | `0@` | 5454, 5455 |
| `$gen402gen` | `0@` | 5456, 5457 |
| `$gen403gen` | `0@` | 5458, 5459 |
| `$gen404gen` | `0@` | 5460, 5461 |
| `$gen405gen` | `0@` | 5462, 5463 |
| `$gen406gen` | `0@` | 5464, 5465 |
| `$gen407gen` | `0@` | 5466, 5467 |
| `$gen408gen` | `0@` | 5468, 5469 |
| `$gen409gen` | `0@` | 5470, 5471 |
| `$gen410gen` | `0@` | 5472, 5473 |
| `$gen411gen` | `0@` | 5474, 5475 |
| `$gen412gen` | `0@` | 5476, 5477 |
| `$gen413gen` | `0@` | 5478, 5479 |
| `$gen414gen` | `0@` | 5480, 5481 |
| `$gen415gen` | `0@` | 5482, 5483 |
| `$gen416gen` | `0@` | 5484, 5485 |
| `$gen417gen` | `0@` | 5486, 5487 |
| `$gen418gen` | `0@` | 5488, 5489 |
| `$gen419gen` | `0@` | 5490, 5491 |
| `$gen420gen` | `0@` | 5492, 5493 |
| `$gen421gen` | `0@` | 5494, 5495 |
| `$gen422gen` | `0@` | 5496, 5497 |
| `$gen423gen` | `0@` | 5498, 5499 |
| `$gen424gen` | `0@` | 5500, 5501 |
| `$gen425gen` | `0@` | 5502, 5503 |
| `$gen426gen` | `0@` | 5504, 5505 |
| `$gen427gen` | `0@` | 5506, 5507 |
| `$gen428gen` | `0@` | 5508, 5509 |
| `$gen429gen` | `0@` | 5510, 5511 |
| `$gen430gen` | `0@` | 5512, 5513 |
| `$gen431gen` | `0@` | 5514, 5515 |
| `$gen432gen` | `0@` | 5516, 5517 |
| `$gen433gen` | `0@` | 5518, 5519 |
| `$gen434gen` | `0@` | 5520, 5521 |
| `$gen435gen` | `0@` | 5522, 5523 |
| `$gen436gen` | `0@` | 5524, 5525 |
| `$gen437gen` | `0@` | 5526, 5527 |
| `$gen438gen` | `0@` | 5528, 5529 |
| `$gen439gen` | `0@` | 5530, 5531 |
| `$gen440gen` | `0@` | 5532, 5533 |
| `$gen441gen` | `0@` | 5534, 5535 |
| `$gen442gen` | `0@` | 5536, 5537 |
| `$gen443gen` | `0@` | 5538, 5539 |
| `$gen444gen` | `0@` | 5540, 5541 |
| `$gen445gen` | `0@` | 5542, 5543 |
| `$gen446gen` | `0@` | 5544, 5545 |
| `$gen447gen` | `0@` | 5546, 5547 |
| `$gen448gen` | `0@` | 5548, 5549 |
| `$gen449gen` | `0@` | 5550, 5551 |
| `$gen450gen` | `0@` | 5552, 5553 |
| `$gen451gen` | `0@` | 5554, 5555 |
| `$gen452gen` | `0@` | 5556, 5557 |
| `$gen453gen` | `0@` | 5558, 5559 |
| `$gen454gen` | `0@` | 5560, 5561 |
| `$gen455gen` | `0@` | 5562, 5563 |
| `$gen456gen` | `0@` | 5564, 5565 |
| `$gen457gen` | `0@` | 5566, 5567 |
| `$gen458gen` | `0@` | 5568, 5569 |
| `$gen459gen` | `0@` | 5570, 5571 |
| `$gen460gen` | `0@` | 5572, 5573 |
| `$gen461gen` | `0@` | 5574, 5575 |
| `$gen462gen` | `0@` | 5576, 5577 |
| `$gen463gen` | `0@` | 5578, 5579 |
| `$gen464gen` | `0@` | 5580, 5581 |
| `$gen465gen` | `0@` | 5582, 5583 |
| `$gen466gen` | `0@` | 5584, 5585 |
| `$gen467gen` | `0@` | 5586, 5587 |
| `$gen468gen` | `0@` | 5588, 5589 |
| `$gen469gen` | `0@` | 5590, 5591 |
| `$gen470gen` | `0@` | 5592, 5593 |
| `$gen471gen` | `0@` | 5594, 5595 |
| `$gen472gen` | `0@` | 5596, 5597 |
| `$gen473gen` | `0@` | 5598, 5599 |
| `$gen474gen` | `0@` | 5600, 5601 |
| `$gen475gen` | `0@` | 5602, 5603 |
| `$gen476gen` | `0@` | 5604, 5605 |
| `$gen477gen` | `0@` | 5606, 5607 |
| `$gen478gen` | `0@` | 5608, 5609 |
| `$gen479gen` | `0@` | 5610, 5611 |
| `$gen480gen` | `0@` | 5612, 5613 |
| `$gen481gen` | `0@` | 5614, 5615 |
| `$gen482gen` | `0@` | 5616, 5617 |
| `$gen483gen` | `0@` | 5618, 5619 |
| `$gen484gen` | `0@` | 5620, 5621 |
| `$gen485gen` | `0@` | 5622, 5623 |
| `$gen486gen` | `0@` | 5624, 5625 |
| `$gen487gen` | `0@` | 5626, 5627 |
| `$gen488gen` | `0@` | 5628, 5629 |
| `$gen489gen` | `0@` | 5630, 5631 |
| `$gen490gen` | `0@` | 5632, 5633 |
| `$gen491gen` | `0@` | 5634, 5635 |
| `$gen492gen` | `0@` | 5636, 5637 |
| `$gen493gen` | `0@` | 5638, 5639 |
| `$gen494gen` | `0@` | 5640, 5641 |
| `$gen495gen` | `0@` | 5642, 5643 |
| `$gen496gen` | `0@` | 5644, 5645 |
| `$gen497gen` | `0@` | 5646, 5647 |
| `$gen498gen` | `0@` | 5648, 5649 |
| `$gen499gen` | `0@` | 5650, 5651 |
| `$gen500gen` | `0@` | 5652, 5653 |
| `$gen501gen` | `0@` | 5654, 5655 |
| `$gen502gen` | `0@` | 5656, 5657 |
| `$gen503gen` | `0@` | 5658, 5659 |
| `$gen504gen` | `0@` | 5660, 5661 |
| `$gen505gen` | `0@` | 5662, 5663 |
| `$gen506gen` | `0@` | 5664, 5665 |
| `$gen507gen` | `0@` | 5666, 5667 |
| `$gen508gen` | `0@` | 5668, 5669 |
| `$gen509gen` | `0@` | 5670, 5671 |
| `$gen510gen` | `0@` | 5672, 5673 |
| `$gen511gen` | `0@` | 5674, 5675 |
| `$gen512gen` | `0@` | 5676, 5677 |
| `$gen513gen` | `0@` | 5678, 5679 |
| `$gen514gen` | `0@` | 5680, 5681 |
| `$gen515gen` | `0@` | 5682, 5683 |
| `$gen516gen` | `0@` | 5684, 5685 |
| `$gen517gen` | `0@` | 5686, 5687 |
| `$gen518gen` | `0@` | 5688, 5689 |
| `$gen519gen` | `0@` | 5690, 5691 |
| `$gen520gen` | `0@` | 5692, 5693 |
| `$gen521gen` | `0@` | 5694, 5695 |
| `$gen522gen` | `0@` | 5696, 5697 |
| `$gen523gen` | `0@` | 5698, 5699 |
| `$gen524gen` | `0@` | 5700, 5701 |
| `$gen525gen` | `0@` | 5702, 5703 |
| `$gen526gen` | `0@` | 5704, 5705 |
| `$gen527gen` | `0@` | 5706, 5707 |
| `$gen528gen` | `0@` | 5708, 5709 |
| `$gen529gen` | `0@` | 5710, 5711 |
| `$gen530gen` | `0@` | 5712, 5713 |
| `$gen531gen` | `0@` | 5714, 5715 |
| `$gen532gen` | `0@` | 5716, 5717 |
| `$gen533gen` | `0@` | 5718, 5719 |
| `$gen534gen` | `0@` | 5720, 5721 |
| `$gen535gen` | `0@` | 5722, 5723 |
| `$gen536gen` | `0@` | 5724, 5725 |
| `$gen537gen` | `0@` | 5726, 5727 |
| `$gen538gen` | `0@` | 5728, 5729 |
| `$gen539gen` | `0@` | 5730, 5731 |
| `$gen540gen` | `0@` | 5732, 5733 |
| `$gen541gen` | `0@` | 5734, 5735 |

### Hilo EAT (zombis de delante)

Locales libres antes del cambio: 0@, 4@, 8@, 9@, 10@, 11@, 12@, 13@, 14@, 15@.
Globales que arrastran valor entre vueltas (ver regla 4): `$205`, `$206`, `$207`, `$208`, `$209`, `$210`, `$211`, `$212`, `$213`.

| Global | Ahora | Líneas |
|---|---|---|
| `$204` | `0@` | 318, 405, 407, 732, 738, 744, 750, 756, 762, 768, 774, 780, 786, 795, 801, 808, 814, 820, 826, 832, 838, 844, 850 |
| `$205` | `4@` | 734, 797 |
| `$206` | `8@` | 740, 803 |
| `$207` | `9@` | 746, 810 |
| `$208` | `10@` | 752, 816 |
| `$209` | `11@` | 758, 822 |
| `$210` | `12@` | 764, 828 |
| `$211` | `13@` | 770, 834 |
| `$212` | `14@` | 776, 840 |
| `$213` | `15@` | 782, 846 |

### Hilo MEAT (zombis de detrás)

Locales libres antes del cambio: 0@, 4@, 8@, 9@, 10@, 11@, 12@, 13@, 14@, 15@.
Globales que arrastran valor entre vueltas (ver regla 4): `$258`, `$259`, `$260`, `$265`, `$266`, `$267`.

| Global | Ahora | Líneas |
|---|---|---|
| `$257` | `0@` | 413, 500, 502, 860, 866, 872, 878, 884, 890, 900, 906, 912, 918, 924, 930 |
| `$258` | `4@` | 862, 902 |
| `$259` | `8@` | 868, 908 |
| `$260` | `9@` | 874, 914 |
| `$265` | `10@` | 880, 920 |
| `$266` | `11@` | 886, 926 |
| `$267` | `12@` | 892, 932 |

### Hilo BEAT

Locales libres antes del cambio: 3@, 4@, 8@, 9@, 10@, 11@, 12@, 13@, 14@, 15@.
Globales que arrastran valor entre vueltas (ver regla 4): `$279`, `$280`, `$281`, `$282`.

| Global | Ahora | Líneas |
|---|---|---|
| `$278` | `3@` | 509, 587, 589, 940, 946, 952, 958, 968, 974, 980, 986 |
| `$279` | `4@` | 942, 970 |
| `$280` | `8@` | 948, 976 |
| `$281` | `9@` | 954, 982 |
| `$282` | `10@` | 960, 988 |

### Hilo DRIVE

Locales libres antes del cambio: 0@, 1@, 2@, 3@, 4@, 5@, 6@, 7@, 8@, 9@, 10@, 11@, 12@, 13@, 14@, 15@.

| Global | Ahora | Líneas |
|---|---|---|
| `$739` | `0@` | 4432, 4433, 4482, 4483 |
| `$740` | `1@` | 4432, 4433, 4482, 4483 |
| `$741` | `2@` | 4432, 4433, 4482, 4483 |

### Hilo BUST

Locales libres antes del cambio: 0@, 1@, 2@, 3@, 4@, 5@, 6@, 7@, 8@, 9@, 10@, 11@, 12@, 13@, 14@, 15@.

| Global | Ahora | Líneas |
|---|---|---|
| `$767` | `0@` | 4094, 4102 |

### Hilo PSAVE1 (punto de guardado)

Locales libres antes del cambio: 0@, 1@, 2@, 3@, 4@, 5@, 6@, 7@, 8@, 9@, 10@, 11@, 12@, 13@, 14@, 15@.
Globales que arrastran valor entre vueltas (ver regla 4): `$2100`.

| Global | Ahora | Líneas |
|---|---|---|
| `$2100` | `0@` | 3876, 3881, 3887, 3888, 3909 |

### Misión 18 — DIABETIC (código en LEGS_26)

Locales libres antes del cambio: 2@, 3@, 4@, 5@, 6@, 7@, 8@, 9@, 10@, 11@, 12@, 13@, 14@, 15@.

| Global | Ahora | Líneas |
|---|---|---|
| `$1370` | `2@` | 3748, 3749, 3750, 3751, 3752, 3753, 3754, 3755, 3756, 3757, 17083, 17084, 17085, 17086, 17087, 17088, 17089, 17090, 17091, 17092 |
| `$1371` | `3@` | 3748, 3749, 3750, 3751, 3752, 3753, 3754, 3755, 3756, 3757, 17083, 17084, 17085, 17086, 17087, 17088, 17089, 17090, 17091, 17092 |
| `$1372` | `4@` | 3748, 3749, 3750, 3751, 3752, 3753, 3754, 3755, 3756, 3757, 17083, 17084, 17085, 17086, 17087, 17088, 17089, 17090, 17091, 17092 |

## Opcodes que ya estaban mezclados en el original (no se tocaron)

Estas 34 líneas ya usaban un opcode de locales con una global, o al revés, antes de este cambio. No forman parte de esta conversión y se dejaron tal cual: el motor de VC acepta cualquiera de los dos tipos de variable en esos parámetros, así que hoy funcionan. Si se quieren dejar limpias, se puede hacer aparte.

| Línea | Código | Opcode según la documentación |
|---|---|---|
| 6283 | `0004: 2@ = 2` | SET_VAR_INT |
| 6513 | `000B: $37 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 6514 | `000B: $38 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 6527 | `000B: $37 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 6528 | `000B: $38 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 6540 | `000B: $37 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 6541 | `000B: $38 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 6579 | `000B: $37 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 6580 | `000B: $38 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 6595 | `000B: $37 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 6596 | `000B: $38 += $706` | ADD_VAL_TO_FLOAT_LVAR |
| 7473 | `0004: 11@ = 4` | SET_VAR_INT |
| 7744 | `0004: 12@ = 5` | SET_VAR_INT |
| 7934 | `0004: 13@ = 1` | SET_VAR_INT |
| 7935 | `0004: 14@ = 0` | SET_VAR_INT |
| 7985 | `0004: 4@ = 6` | SET_VAR_INT |
| 12367 | `000B: 1@ += 6@` | ADD_VAL_TO_FLOAT_LVAR |
| 12368 | `000B: 2@ += 5@` | ADD_VAL_TO_FLOAT_LVAR |
| 14487 | `0004: 8@ = 0` | SET_VAR_INT |
| 14488 | `0004: 9@ = 0` | SET_VAR_INT |
| 14489 | `0004: 10@ = 0` | SET_VAR_INT |
| 14490 | `0004: 11@ = 0` | SET_VAR_INT |
| 14505 | `8038:   not 8@ == 1` | IS_INT_VAR_EQUAL_TO_NUMBER |
| 14511 | `0004: 8@ = 1` | SET_VAR_INT |
| 14521 | `8038:   not 9@ == 1` | IS_INT_VAR_EQUAL_TO_NUMBER |
| 14527 | `0004: 9@ = 1` | SET_VAR_INT |
| 14537 | `8038:   not 10@ == 1` | IS_INT_VAR_EQUAL_TO_NUMBER |
| 14543 | `0004: 10@ = 1` | SET_VAR_INT |
| 14553 | `8038:   not 11@ == 1` | IS_INT_VAR_EQUAL_TO_NUMBER |
| 14559 | `0004: 11@ = 1` | SET_VAR_INT |
| 15408 | `0009: 0@ += -3.0` | ADD_VAL_TO_FLOAT_VAR |
| 15507 | `0022:   $1179 > 0@` | IS_NUMBER_GREATER_THAN_FLOAT_VAR |
| 15672 | `0009: 0@ += -3.0` | ADD_VAL_TO_FLOAT_VAR |
| 15771 | `0022:   $1179 > 0@` | IS_NUMBER_GREATER_THAN_FLOAT_VAR |

## Cómo probarlo

1. Compilar `MAIN_fixed.txt` con Sanny Builder (modo Vice City), como siempre.
2. **Empezar una partida nueva.** Las partidas guardadas con el `main.scm` anterior no son compatibles con uno modificado.
3. Guardar en los dos puntos de guardado (el de `PSAVE1` en -65.9, 1025.0 y el de BIG1 en -49.2, 1000.8) y volver a cargar cada partida.
