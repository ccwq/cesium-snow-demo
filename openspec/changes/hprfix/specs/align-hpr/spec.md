## ADDED Requirements
### Requirement: Earth-Aligned HPR And Drift
The system SHALL derive camera heading/pitch/roll and snow drift vectors from the Cesium local East-North-Up frame so that the gravity-aligned direction always points toward the ellipsoid normal regardless of hemisphere or latitude.

#### Scenario: Drift stays downward across hemispheres
- **WHEN** the Cesium camera moves from a northern latitude to a southern latitude
- **THEN** the snow particle downward vector continues to follow the local ellipsoid normal without flipping upward

#### Scenario: Polar stability fallback
- **WHEN** the camera enters polar regions where ENU becomes unstable
- **THEN** the system SHALL clamp or blend orientation to maintain continuous HPR and downward drift

### Requirement: GUI Syncs Earth Pose
The system SHALL expose Cesium camera heading/pitch/roll and local XYZ position in lil-gui, updating values live when the camera changes and applying GUI edits back to the Three.js overlay camera in the same Earth-aligned frame.

#### Scenario: GUI reflects camera movement
- **WHEN** the user or animation changes the Cesium camera pose
- **THEN** the lil-gui HPR/XYZ fields update within one render frame to the matching Earth-aligned values

#### Scenario: GUI edits reapply Earth pose
- **WHEN** the user edits HPR or XYZ in lil-gui
- **THEN** the Three.js camera reorients/relocates using the Earth-aligned frame so snow drift and visuals stay consistent with local gravity
