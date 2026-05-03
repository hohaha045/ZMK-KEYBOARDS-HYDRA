# Hydra Keyboard Firmware
[![Build ZMK firmware](https://github.com/Perseus333/zmk-keyboards-daedalus/actions/workflows/build.yml/badge.svg)](https://github.com/hohaha045/ZMK-KEYBOARDS-HYDRA/actions/workflows/build.yml)

Built with ZMK on GitHub actions. Find the latest binaries at the Releases tab.

TODO: Hydra Firmware Enhancements
==================================

[ ] 1. ENCODER SWAP (EC05E1220202 / CKW12)
        - Same alps,ec11 compatible binding, no driver change needed
        - Update pin assignments in hydra.dtsi:
            left_encoder: encoder { a-gpios = <&pro_micro X ...>; b-gpios = <&pro_micro Y ...>; };
        - No switch-gpios needed unless you want encoder press as a key

[ ] 2. SCROLL MODE (trackpad + trackball)
        - Add zmk,input-processor node: zip_xy_to_scroll_mapper
        - Add a second zmk,input-listener with that processor, activated by a hold-layer key
        - Left (Cirque): glidepoint_listener_scroll / Right (PMW3610): trackball_listener_scroll
        - Both listeners sit on the same device; the active layer determines which fires

[ ] 3. SNIPER MODE
        - Add zmk,input-processor node: zip_xy_scaler with x-scale/y-scale = <1 4> (25% speed)
        - Same pattern as scroll: second listener with that processor, hold-layer key to activate
        - Can stack with scroll mode on a separate layer

[ ] 4. AUTO-MOUSE LAYER
        - Add zmk,auto-layer node pointing at a dedicated mouse layer
        - Any input event from trackball/trackpad auto-activates the layer
        - Layer auto-deactivates after idle-ms of no pointer movement
        - Mouse buttons live on that layer so they disappear when you go back to typing

[ ] 5. CLICK AND DRAG
        - Cirque tap-to-click is already enabled (primary-tap-enable in overlay)
        - Double-tap-and-hold = drag on macOS (enable "tap to click" + "three finger drag" in
          System Settings → Accessibility → Pointer Control)
        - For explicit drag key: bind a key to &mkp LCLK on the mouse layer, hold it + move

[ ] 6. 3D TRACKBALL NAVIGATION (Blender / Fusion 360)
        - Three input-listeners on the PMW3610, each on a different layer:
            trackball_listener        → default: REL_X / REL_Y (normal cursor)
            trackball_listener_orbit  → zip_xy_to_scroll_mapper: Y→REL_WHEEL, X→REL_HWHEEL
            trackball_listener_rotate → zip_temp_layer or zip_xy_scaler X-only (constrained)
        - Hold "Orbit" key  → activates orbit layer  → ball scrolls/zooms model
        - Hold "Pan" key    → activates pan layer    → ball pans viewport
        - Blender default: middle-mouse = orbit, Shift+middle = pan, Ctrl+middle = zoom
          Map orbit key to also send middle-mouse-button held via &mkp MCLK
