/*
 * Copyright (c) 2020 The ZMK Contributors
 *
 * SPDX-License-Identifier: MIT
 */

#include <behaviors.dtsi>
#include <dt-bindings/zmk/keys.h>
#include <dt-bindings/zmk/bt.h>
#include <dt-bindings/zmk/rgb.h>
#include <dt-bindings/zmk/ext_power.h>

#define BASE 0
#define LOWER 1
#define RAISE 2
#define ADJUST 3

/ {

   // Activate ADJUST layer by pressing raise and lower
    conditional_layers {
        compatible = "zmk,conditional-layers";
        adjust_layer {
            if-layers = <LOWER RAISE>;
            then-layer = <ADJUST>;
        };
    };

    keymap {
        compatible = "zmk,keymap";

        default_layer {
            label = "default";
// ------------------------------------------------------------------------------------------------------------
// |   `   |  1  |  2  |  3   |  4   |  5   |                   |  6   |  7    |  8    |  9   |   0   |       |
// |  ESC  |  Q  |  W  |  E   |  R   |  T   |                   |  Y   |  U    |  I    |  O   |   P   | BKSPC |
// |  TAB  |  A  |  S  |  D   |  F   |  G   |                   |  H   |  J    |  K    |  L   |   ;   |   '   |
// | SHIFT |  Z  |  X  |  C   |  V   |  B   |  MUTE  |  |       |  N   |  M    |  ,    |  .   |   /   | SHIFT |
//               | GUI | ALT  | CTRL | LOWER|  ENTER |  | SPACE | RAISE| CTRL  | ALT   | GUI  |
            bindings = <
&kp ESC &kp N1 &kp N2   &kp N3   &kp N4    &kp N5                           &kp N6 &kp N7    &kp N8    &kp N9   &kp N0   &kp BSPC
&kp TAB   &kp Q  &kp W    &kp E    &kp R     &kp T                            &kp Y  &kp U     &kp I     &kp O    &kp P    &kp CLCK
&kp GRAVE   &kp A  &kp S    &kp D    &kp F     &kp G                            &kp H  &kp J     &kp K     &kp L    &kp SEMI &kp SQT
&kp LCTRL &kp Z  &kp X    &kp C    &kp V     &kp B      &kp C_MUTE &none      &kp N  &kp M     &kp COMMA &kp DOT  &kp FSLH &kp RCTRL
                 &kp LGUI &kp LALT &kp SHFT &mo LOWER  &kp SPACE    &kp RET  &mo RAISE  &kp RSHFT &kp RALT  &kp RGUI
            >;

            sensor-bindings = <&inc_dec_kp C_VOL_UP C_VOL_DN &inc_dec_kp PG_UP PG_DN>;
        };

        lower_layer {
            label = "lower";
// TODO: Some binds are waiting for shifted keycode support.
// ------------------------------------------------------------------------------------------------------------
// |       |  F1 |  F2 |  F3  |  F4  |  F5  |                   |  F6  |  F7   |  F8   |  F9  |  F10  |  F11  |
// |   `   |  1  |  2  |  3   |  4   |  5   |                   |  6   |  7    |  8    |  9   |   0   |  F12  |
// |       |  !  |  @  |  #   |  $   |  %   |                   |  ^   |  &    |  *    |  (   |   )   |   |   |
// |       |  =  |  -  |  +   |  {   |  }   |        |  |       |  [   |  ]    |  ;    |  :   |   \   |       |
//               |     |      |      |      |        |  |       |      |       |       |      |
            bindings = <
&trans    &kp F1    &kp F2    &kp F3      &kp F4    &kp F5                    &kp F6    &kp F7   &kp F8          &kp F9    &kp F10   &kp F11
&kp GRAVE &kp N1    &kp N2    &kp N3      &kp N4    &kp N5                    &kp N6    &kp N7   &kp N8          &kp N9    &kp N0    &kp F12
&trans    &kp EXCL  &kp AT    &kp HASH    &kp DLLR  &kp PRCNT                 &kp CARET &kp AMPS &kp KP_MULTIPLY &kp LPAR  &kp RPAR  &kp PIPE
&trans    &kp EQUAL &kp MINUS &kp KP_PLUS &kp LBRC  &kp RBRC   &trans &trans  &kp LBKT  &kp RBKT &kp SEMI        &kp COLON &kp BSLH  &trans
                    &trans    &trans      &trans    &trans     &trans &trans  &trans    &trans   &trans          &trans
            >;

            sensor-bindings = <&inc_dec_kp C_VOL_UP C_VOL_DN &inc_dec_kp PG_UP PG_DN>;
        };

        raise_layer {
            label = "raise";
// ------------------------------------------------------------------------------------------------------------
// | BTCLR | BT1  | BT2  |  BT3  |  BT4  |  BT5 |                |      |      |       |      |       |       |
// |       | INS  | PSCR | GUI   |       |      |                | PGUP |      |   ^   |      |       |       |
// |       | ALT  | CTRL | SHIFT |       | CAPS |                | PGDN |   <- |   v   |  ->  |  DEL  | BKSPC |
// |       | UNDO | CUT  | COPY  | PASTE |      |      |  |      |      |      |       |      |       |       |
//                |      |       |       |      |      |  |      |      |      |       |      |
            bindings = <
&bt BT_CLR &bt BT_SEL 0 &bt BT_SEL 1 &bt BT_SEL 2 &bt BT_SEL 3 &bt BT_SEL 4                  &trans    &trans    &trans   &trans    &trans  &trans
&trans     &kp INS      &kp PSCRN    &kp K_CMENU  &trans       &trans                        &kp PG_UP &trans    &kp UP   &trans    &kp N0  &trans
&trans     &kp LALT     &kp LCTRL    &kp LSHFT    &trans       &kp CLCK                      &kp PG_DN &kp LEFT  &kp DOWN &kp RIGHT &kp DEL &kp BSPC
&trans     &kp K_UNDO   &kp K_CUT    &kp K_COPY   &kp K_PASTE  &trans        &trans  &trans  &trans    &trans    &trans   &trans    &trans  &trans
                        &trans       &trans       &trans       &trans        &trans  &trans  &trans    &trans    &trans   &trans
            >;

            sensor-bindings = <&inc_dec_kp C_VOL_UP C_VOL_DN &inc_dec_kp PG_UP PG_DN>;
        };

        adjust_layer {
// ----------------------------------------------------------------------------------------------------------------------------
// | BTCLR  |  BT1    |  BT2    |   BT3   |   BT4   |   BT5   |                  |      |      |       |      |       |       |
// | EXTPWR | RGB_HUD | RGB_HUI | RGB_SAD | RGB_SAI | RGB_EFF |                  |      |      |       |      |       |       |
// |        | RGB_BRD | RGB_BRI |         |         |         |                  |      |      |       |      |       |       |
// |        |         |         |         |         |         | RGB_TOG | |      |      |      |       |      |       |       |
//                    |         |         |         |         |         | |      |      |      |       |      |
            label = "adjust";
            bindings = <
&bt BT_CLR        &bt BT_SEL 0    &bt BT_SEL 1    &bt BT_SEL 2    &bt BT_SEL 3    &bt BT_SEL 4                            &none &none &none &none &none &none
&ext_power EP_TOG &rgb_ug RGB_HUD &rgb_ug RGB_HUI &rgb_ug RGB_SAD &rgb_ug RGB_SAI &rgb_ug RGB_EFF                         &none &none &none &none &none &none
&none             &rgb_ug RGB_BRD &rgb_ug RGB_BRI &none           &none           &none                                   &none &none &none &none &none &none
&none             &none           &none           &none           &none           &none            &rgb_ug RGB_TOG &none  &none &none &none &none &none &none
                                  &none           &none           &none           &none            &none           &none  &none &none &none &none
            >;
        };

    };
};
