# SqueezeTheZombies
Android game created in Game Maker Studio in 2015. Smash zombies to survive ;)

**Apk is for test use only!**

Project excludes *sprites* and *sound* folder.

Mostly using GML language, not GUI. Sprites assets was bought on HumbleBundle but now license is deprecated ([only RPGMaker use only](https://support.humblebundle.com/hc/en-us/articles/206156508))
# **Game is not released!**

Screenshots:

![Screenshot1](https://github.com/wboniecki/SqueezeTheZombies/blob/master/Screenshot_20170712-235933.png)
![Screenshot2](https://github.com/wboniecki/SqueezeTheZombies/blob/master/Screenshot_20170712-235911.png)
![Screenshot3](https://github.com/wboniecki/SqueezeTheZombies/blob/master/Screenshot_20170712-235940.png)


**Example code:**

## Drawing ambient light and point lights in view
```
///DrawTheDarkAndLight
if(surface_exists(surf)) {
    surface_set_target(surf);
    draw_set_colour(c_black);
    draw_set_alpha(0.95);
    draw_rectangle(0,0,room_width,room_height,false);
    
    //set circles
    draw_set_blend_mode(bm_subtract);
    texture_set_interpolation(true);
    
    //draw circle
// with(obj_Target) {      
// draw_circle(x+12, y+17,60, false);
// test = random_range(-0.05,0.05);
// draw_sprite_ext(spr_white_big_glow,0,x+12,y+17,1,1,0,c_yellow,0.6+test);
// draw_sprite_ext(spr_white_big_glow,0,x+12,y+17,1.5,1.5,0,c_yellow,0.3+test);
// draw_sprite_ext(spr_white_big_glow,0,x+12,y+17,2,2,0,c_yellow,0.1+test);
// for(i=0;i<=7;i+=1){
// draw_set_alpha(0+(0.1*i));
// draw_circle(x+12, y+17,45-(i*3), false);
// }
// }
    draw_set_blend_mode(bm_inv_src_colour)
    with(obj_Lamp) {
        test = random_range(-0.05,0.05);
        draw_sprite_ext(spr_white_big_glow,0,x,y,1,1,0,c_orange,0.6+test);
        draw_sprite_ext(spr_white_big_glow,0,x+12,y+17,1.5,1.5,0,c_orange,0.3+test);
        draw_sprite_ext(spr_white_big_glow,0,x+12,y+17,2,2,0,c_orange,0.1+test);
    }

    
    //reset
    draw_set_blend_mode(bm_normal);
    texture_set_interpolation(false);
    //draw_set_alpha(1);
    surface_reset_target();
    
    
} else {
        surf = surface_create(room_width, room_height);
        surface_set_target(surf);
        draw_clear_alpha(c_black, 0);
        surface_reset_target();
    }
```

## Enemy handler
```
/// OnCreate
isSpawn = false;
spawnTime = random(100);
alarm[0] = spawnTime;

/// Spawner
if(obj_Target.isAlive && isSpawn){
    instance_create(x,y,obj_Enemy);
    time = 120 - obj_Target.combo;
    if(time <= 30) {
        time = 30;
    }
    spawnTime = time;
}
alarm[0] = spawnTime;

if(mouse_check_button(mb_left) && !isSpawn) {
    isSpawn = true;
    button = audio_play_sound(s_button,1,false);
    audio_sound_gain(button,0.1,0);
}
```

## Drawing game HUD
```
///DrawGameHUD
if(displayGUI){
    draw_set_colour(c_white);
    draw_set_font(PixelFontScore);
    //draw start at beggining
    if(!obj_Enemy_Handler.isSpawn){
        draw_set_halign(fa_center);
        draw_text(display_get_gui_width()/2, display_get_gui_height()/2,"SMASH!");
    }
    draw_set_halign(fa_left);
    display_set_gui_size(800,480);
    // draw score
    draw_text_transformed(15,15,score,1,1,0);
    //loop for draw hp
    for(i=0;i<obj_Target.targetHP;i+=1){
        draw_sprite(spr_Heart,0,display_get_gui_width()-50 -(i*50),display_get_gui_height()-50);
    }
    //draw combo
    draw_set_halign(fa_center);
    if(obj_Target.combo < 15) {
            draw_set_colour(c_white);
            draw_set_font(PixelFontComboSmall);
    }
    if(obj_Target.combo > 15 && obj_Target.combo < 50) {
            draw_set_colour(c_white);
            draw_set_font(PixelFontScore);
    }
    if(obj_Target.combo >= 50)
    {
            draw_set_colour(c_red);
            draw_set_font(PixelFontComboBig);
    }
    if(obj_Target.combo > 0) {
    draw_text_transformed(display_get_gui_width()/2,display_get_gui_height()-100,"+"+string(obj_Target.combo),1+offset,1+offset,0);
    }
}

//DrawEndGameHUD
if(!displayGUI) {
    draw_set_colour(c_white);
    draw_set_font(PixelFontScore);
    draw_set_halign(fa_center);
    draw_text(display_get_gui_width()/2 , 20,"SCORE");
    draw_text(display_get_gui_width()/2 , 100,score);
    draw_set_font(PixelFontComboSmall);
    draw_text(display_get_gui_width()/2, display_get_gui_height()-180,"highscore: "+string(global.highscore));
    draw_set_font(PixelFontScore);
    draw_set_alpha(alpha);
    draw_text(display_get_gui_width()/2, display_get_gui_height()-100,"tap to restart!");
    draw_set_alpha(1); 
}
      
```
