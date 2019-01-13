# v-besttorso

This resource aims to help people find proper torsos for a freemode character top item (component ID 11). So this ugly clipping doesn't happen:

![Disgusting!](https://raw.githubusercontent.com/root-cause/v-besttorso/master/ClippingExample.jpg)

This resource only provides "the best" torso for a specified top item, it will not help with:

* Gloves
* Undershirts (component ID 8)

Some tops don't have torso data so they'll just return -1, a better explanation can be found on the README file of [v-clothingnames](https://github.com/root-cause/v-clothingnames).

Visit https://wiki.rage.mp/index.php?title=Clothes for preview images of tops and torsos.

# JS Example

```js
const torsoDataMale = require("./besttorso_male.json");
const torsoDataFemale = require("./besttorso_female.json");
const freemodeModels = [ mp.joaat("mp_m_freemode_01"), mp.joaat("mp_f_freemode_01") ];

mp.events.addCommand("settop", (player, _, drawable, texture) => {
    if (freemodeModels.includes(player.model)) {
        drawable = Number(drawable);
        texture = Number(texture);

        if (isNaN(drawable) || isNaN(texture)) {
            player.outputChatBox("SYNTAX: /settop [drawable] [texture]");
        } else {
            if (player.model == freemodeModels[0]) {
                // male
                if (torsoDataMale[drawable] === undefined || torsoDataMale[drawable][texture] === undefined) {
                    player.outputChatBox("Invalid top drawable/texture.");
                } else {
                    player.setClothes(11, drawable, texture, 2);
                    if (torsoDataMale[drawable][texture] != -1) player.setClothes(3, torsoDataMale[drawable][texture].BestTorsoDrawable, torsoDataMale[drawable][texture].BestTorsoTexture, 2);
                }
            } else {
                // female
                if (torsoDataFemale[drawable] === undefined || torsoDataFemale[drawable][texture] === undefined) {
                    player.outputChatBox("Invalid top drawable/texture.");
                } else {
                    player.setClothes(11, drawable, texture, 2);
                    if (torsoDataFemale[drawable][texture] != -1) player.setClothes(3, torsoDataFemale[drawable][texture].BestTorsoDrawable, torsoDataFemale[drawable][texture].BestTorsoTexture, 2);
                }
            }
        }
    } else {
        player.outputChatBox("Switch to a freemode model first.");
    }
});
```
