# go4vl Roadmap - V4L2 API Parity

This is the primary roadmap for go4vl, tracking implementation of V4L2 (Video for Linux 2) API features based on the official Linux kernel documentation structure: [Video for Linux API](https://www.kernel.org/doc/html/latest/userspace-api/media/v4l/v4l2.html)

**Primary Goal**: Achieve feature parity with the V4L2 userspace API, providing idiomatic Go bindings for all major V4L2 capabilities.

**See Also**: [ROADMAP_EXTENDED.md](./ROADMAP_EXTENDED.md) for strategic initiatives and performance optimization efforts beyond V4L2 parity.

**Legend**:
- ✅ Complete
- 🚧 Partial / In Progress
- ❌ Not Started
- 🔴 Out of Scope (kernel-specific/deprecated)

---

## 1. Common API Elements

### 1.1 Opening and Closing Devices
**Status**: ✅ Complete

- [x] Device opening (`device.Open()`)
- [x] Device closing (`device.Close()`)
- [x] Multiple file descriptor management
- [x] Device node naming conventions
- [x] Error handling

**Files**: `device/device.go`, `v4l2/syscalls.go`

---

### 1.2 Querying Capabilities
**Status**: ✅ Complete

- [x] `VIDIOC_QUERYCAP` - Query device capabilities
- [x] Capability struct (`v4l2.Capability`)
- [x] Capability flag checking methods
- [x] Driver/device/bus information

**Files**: `v4l2/capability.go`, `device/device.go`

**Tests**: `v4l2/capability_test.go`

---

### 1.3 Application Priority
**Status**: ❌ Not Started

- [ ] `VIDIOC_G_PRIORITY` - Get access priority
- [ ] `VIDIOC_S_PRIORITY` - Set access priority
- [ ] Priority levels (background, interactive, record)
- [ ] Priority conflict resolution

**Priority**: Low (rarely used in practice)

**Deliverables**:
- Add priority constants to `v4l2/types.go`
- Implement `GetPriority()` and `SetPriority()` methods
- Add examples showing multi-application coordination

---

### 1.4 Video Inputs and Outputs
**Status**: 🚧 Partial

- [x] `VIDIOC_ENUMINPUT` - Enumerate video inputs
- [x] `VIDIOC_G_INPUT` - Get current input
- [x] `VIDIOC_S_INPUT` - Set current input
- [ ] `VIDIOC_ENUMOUTPUT` - Enumerate video outputs
- [ ] `VIDIOC_G_OUTPUT` - Get current output
- [ ] `VIDIOC_S_OUTPUT` - Set current output
- [ ] Input/output status queries
- [ ] Audio/video standard association

**Files**: `device/device.go` (partial)

**Deliverables**:
- Complete output device support
- Add input/output enumeration
- Add status query methods
- Create video output examples

---

### 1.5 Audio Inputs and Outputs
**Status**: ❌ Not Started

- [ ] `VIDIOC_ENUMAUDIO` - Enumerate audio inputs
- [ ] `VIDIOC_G_AUDIO` - Get current audio input
- [ ] `VIDIOC_S_AUDIO` - Set current audio input
- [ ] `VIDIOC_ENUMAUDOUT` - Enumerate audio outputs
- [ ] `VIDIOC_G_AUDOUT` - Get current audio output
- [ ] `VIDIOC_S_AUDOUT` - Set current audio output
- [ ] Audio capability flags
- [ ] Audio mode selection

**Priority**: Medium (needed for TV tuner cards, webcams with mics)

**Deliverables**:
- Create `v4l2/audio.go` with audio structures
- Implement audio enumeration and selection
- Add audio capability queries
- Create example with webcam microphone

---

### 1.6 Tuners and Modulators
**Status**: ❌ Not Started

- [ ] `VIDIOC_G_TUNER` - Get tuner properties
- [ ] `VIDIOC_S_TUNER` - Set tuner properties
- [ ] `VIDIOC_G_MODULATOR` - Get modulator properties
- [ ] `VIDIOC_S_MODULATOR` - Set modulator properties
- [ ] `VIDIOC_G_FREQUENCY` - Get tuner/modulator frequency
- [ ] `VIDIOC_S_FREQUENCY` - Set tuner/modulator frequency
- [ ] `VIDIOC_ENUM_FREQ_BANDS` - Enumerate frequency bands
- [ ] Tuner capabilities and modes
- [ ] Signal strength/AFC monitoring
- [ ] RDS/RBDS support

**Priority**: Low (niche hardware - TV tuners, SDR)

**Deliverables**:
- Create `v4l2/tuner.go`
- Implement tuner/modulator structures
- Add frequency control methods
- Create FM radio tuner example
- Create TV tuner example

---

### 1.7 Video Standards
**Status**: ❌ Not Started

- [ ] `VIDIOC_ENUMSTD` - Enumerate video standards
- [ ] `VIDIOC_G_STD` - Get current standard
- [ ] `VIDIOC_S_STD` - Set standard
- [ ] `VIDIOC_QUERYSTD` - Detect standard
- [ ] Standard IDs (PAL, NTSC, SECAM, etc.)
- [ ] Standard framerates and line counts

**Priority**: Low (legacy analog TV, most modern devices use DV timings)

**Deliverables**:
- Create `v4l2/standard.go`
- Add standard enumeration
- Add standard detection
- Document standard selection workflow

---

### 1.8 Digital Video (DV) Timings
**Status**: ❌ Not Started

- [ ] `VIDIOC_ENUM_DV_TIMINGS` - Enumerate DV timings
- [ ] `VIDIOC_G_DV_TIMINGS` - Get current DV timings
- [ ] `VIDIOC_S_DV_TIMINGS` - Set DV timings
- [ ] `VIDIOC_QUERY_DV_TIMINGS` - Detect DV timings
- [ ] `VIDIOC_DV_TIMINGS_CAP` - Get DV timing capabilities
- [ ] Timing presets (CEA-861, DMT, etc.)
- [ ] Custom timing support
- [ ] Interlaced/progressive detection

**Priority**: Medium (needed for HDMI capture cards, professional video)

**Deliverables**:
- Create `v4l2/dv_timings.go`
- Implement timing structures
- Add timing enumeration and detection
- Create HDMI capture example

---

### 1.9 User Controls
**Status**: ✅ Complete

- [x] `VIDIOC_QUERYCTRL` - Query control properties
- [x] `VIDIOC_G_CTRL` - Get control value
- [x] `VIDIOC_S_CTRL` - Set control value
- [x] `VIDIOC_QUERYMENU` - Query menu items
- [x] Control types (integer, boolean, menu, button, etc.)
- [x] Control flags and capabilities

**Files**: `v4l2/control.go`, `device/device.go`

**Tests**: `v4l2/control_test.go`

**Examples**: `examples/user_ctrl/`

---

### 1.10 Extended Controls API
**Status**: 🚧 Partial

- [x] `VIDIOC_G_EXT_CTRLS` - Get extended controls
- [x] `VIDIOC_S_EXT_CTRLS` - Set extended controls
- [x] `VIDIOC_TRY_EXT_CTRLS` - Try extended controls
- [ ] Compound controls (arrays, structs)
- [ ] Control classes (user, codec, camera, etc.)
- [ ] `VIDIOC_SUBSCRIBE_EVENT` - Control change events
- [ ] `VIDIOC_UNSUBSCRIBE_EVENT`
- [ ] `VIDIOC_DQEVENT` - Dequeue control events
- [ ] Atomic multi-control operations

**Files**: `v4l2/control.go` (partial)

**Deliverables**:
- Implement compound control support
- Add control event subscription
- Add control classes/grouping
- Create codec control examples

---

### 1.11-1.15 Control References
**Status**: 🚧 Partial

#### 1.11 Camera Control Reference
- [x] Basic camera controls (exposure, focus, zoom)
- [ ] Complete camera control enumeration
- [ ] Auto-focus regions
- [ ] Scene modes
- [ ] Exposure metering

#### 1.12 Flash Control Reference
- [ ] Flash mode controls
- [ ] Flash intensity
- [ ] Torch mode
- [ ] Flash timing

#### 1.13 Image Source Control Reference
- [ ] Analog gain
- [ ] Digital gain
- [ ] Test patterns

#### 1.14 Image Process Control Reference
- [ ] Color correction
- [ ] Sharpness
- [ ] Noise reduction

#### 1.15 Codec Control Reference
- [ ] H.264 encoder controls
- [ ] H.265/HEVC encoder controls
- [ ] VP8/VP9 encoder controls
- [ ] MPEG controls
- [ ] Bitrate control modes
- [ ] GOP structure
- [ ] Quality/profile presets

**Priority**: High for codec controls, Medium for others

**Deliverables**:
- Create `v4l2/ext_ctrls_camera.go`
- Create `v4l2/ext_ctrls_codec.go`
- Implement codec control profiles
- Create hardware encoder example

---

## 2. Data Formats

### 2.1 Image Formats
**Status**: 🚧 Partial

- [x] `VIDIOC_ENUM_FMT` - Enumerate formats
- [x] `VIDIOC_G_FMT` - Get format
- [x] `VIDIOC_S_FMT` - Set format
- [x] `VIDIOC_TRY_FMT` - Try format
- [x] Pixel format FOURCCs (MJPEG, YUYV, H264, etc.)
- [ ] Format flags and capabilities
- [ ] Colorspace information
- [ ] Quantization range
- [ ] Transfer function (gamma)

**Files**: `v4l2/formats.go`, `device/device.go`

**Deliverables**:
- Complete colorspace support
- Add HDR metadata
- Document all supported pixel formats
- Add format validation utilities

---

### 2.2 Compressed Formats
**Status**: 🚧 Partial

- [x] MJPEG support
- [x] H.264 support
- [ ] H.265/HEVC support
- [ ] VP8/VP9 support
- [ ] MPEG-2/4 support
- [ ] Format-specific metadata

**Files**: `v4l2/formats.go`

**Deliverables**:
- Add missing codec format constants
- Document codec-specific parameters
- Create encoder/decoder examples

---

### 2.3 Reserved Format Identifiers
**Status**: ✅ Complete

- [x] FourCC pixel format codes
- [x] Pixel format constants

**Files**: `v4l2/formats.go`

---

### 2.4 Field Order
**Status**: ✅ Complete

- [x] Progressive
- [x] Interlaced (top/bottom first)
- [x] Field alternate

**Files**: `v4l2/formats.go`

---

### 2.5 Colorspaces
**Status**: ❌ Not Started

- [ ] `V4L2_COLORSPACE_*` constants
- [ ] sRGB, Rec. 709, Rec. 2020
- [ ] YCbCr encoding
- [ ] Quantization ranges
- [ ] Transfer functions
- [ ] Colorspace conversion helpers

**Priority**: Medium (important for professional video, HDR)

**Deliverables**:
- Create `v4l2/colorspace.go`
- Add all colorspace constants
- Add colorspace detection
- Document colorspace workflows

---

## 3. Input/Output Methods

### 3.1 Read/Write
**Status**: ❌ Not Started

- [ ] `read()` syscall support
- [ ] `write()` syscall support
- [ ] Blocking/non-blocking modes
- [ ] `select()` integration

**Priority**: Low (inefficient, streaming preferred)

**Deliverables**:
- Add read/write I/O option
- Create simple read/write example
- Document limitations vs streaming

---

### 3.2 Streaming I/O (Memory Mapping)
**Status**: ✅ Complete

- [x] `VIDIOC_REQBUFS` - Request buffers
- [x] `VIDIOC_QUERYBUF` - Query buffer
- [x] `VIDIOC_QBUF` - Queue buffer
- [x] `VIDIOC_DQBUF` - Dequeue buffer
- [x] `VIDIOC_STREAMON` - Start streaming
- [x] `VIDIOC_STREAMOFF` - Stop streaming
- [x] Memory-mapped buffers (MMAP)
- [x] Buffer lifecycle management

**Files**: `v4l2/streaming.go`, `device/device.go`

**Tests**: `test/integration_test.go`

---

### 3.3 Streaming I/O (User Pointer)
**Status**: ❌ Not Started

- [ ] `V4L2_MEMORY_USERPTR` support
- [ ] User-allocated buffers
- [ ] Buffer passing to driver
- [ ] Memory alignment requirements

**Priority**: Medium (zero-copy integration with other libraries)

**Deliverables**:
- Add UserPtr buffer allocation
- Implement USERPTR queue/dequeue
- Create userptr example
- Benchmark vs MMAP

---

### 3.4 Streaming I/O (DMA Buffer Importing)
**Status**: ❌ Not Started

- [ ] `V4L2_MEMORY_DMABUF` support
- [ ] `VIDIOC_EXPBUF` - Export buffer as DMA-BUF
- [ ] DMA-BUF file descriptor handling
- [ ] Buffer import from DRM/GPU
- [ ] DMA-BUF synchronization

**Priority**: Medium-High (critical for GPU/display pipelines)

**Deliverables**:
- Create `v4l2/dmabuf.go`
- Add DMA-BUF export/import
- Create DRM integration example
- Create GPU zero-copy example

---

### 3.5 Asynchronous I/O
**Status**: ❌ Not Started

- [ ] Non-blocking mode
- [ ] `poll()` / `select()` / `epoll()` integration
- [ ] `VIDIOC_DQEVENT` for async events
- [ ] Timeout handling

**Priority**: Medium (needed for multi-device capture)

**Deliverables**:
- Expose file descriptor for polling
- Add async capture mode
- Create multi-device epoll example

---

### 3.6 Buffers
**Status**: ✅ Complete

- [x] Buffer structure (`v4l2.Buffer`)
- [x] Buffer flags
- [x] Timestamp handling
- [x] Sequence numbers
- [x] Buffer metadata

**Files**: `v4l2/streaming.go`, `device/frame.go`

---

### 3.7 Field Order
**Status**: ✅ Complete

- [x] Field order constants
- [x] Progressive/interlaced detection

**Files**: `v4l2/formats.go`

---

## 4. Interfaces

### 4.1 Video Capture Interface
**Status**: ✅ Complete

- [x] Single-planar capture (`V4L2_BUF_TYPE_VIDEO_CAPTURE`)
- [x] Frame capture loop
- [x] Format negotiation
- [x] Buffer management
- [x] Context-based cancellation

**Files**: `device/device.go`, `device/capture_bytes.go`, `device/capture_frames.go`

**Examples**: `examples/capture0/`, `examples/capture_frames/`

---

### 4.2 Video Capture Interface (Multi-Planar)
**Status**: ❌ Not Started

- [ ] `V4L2_BUF_TYPE_VIDEO_CAPTURE_MPLANE`
- [ ] Multi-planar buffer handling
- [ ] Plane structure support
- [ ] NV12, I420, YV12 planar formats

**Priority**: Medium (needed for many hardware decoders/encoders)

**Deliverables**:
- Add multi-planar buffer types
- Implement plane handling
- Create multi-planar capture example

---

### 4.3 Video Output Interface
**Status**: 🚧 Partial

- [ ] `V4L2_BUF_TYPE_VIDEO_OUTPUT`
- [ ] Frame output loop
- [ ] Output format setup
- [ ] Output buffer queueing
- [ ] Timing control

**Priority**: Medium (needed for video injection, encoding)

**Deliverables**:
- Complete output interface
- Add output streaming loop
- Create video playback example
- Create encoder example

---

### 4.4 Video Output Interface (Multi-Planar)
**Status**: ❌ Not Started

- [ ] `V4L2_BUF_TYPE_VIDEO_OUTPUT_MPLANE`
- [ ] Multi-planar output
- [ ] Plane-based output formats

**Priority**: Low (less common than capture)

---

### 4.5 Video Overlay Interface
**Status**: 🔴 Out of Scope

- Deprecated, not recommended for new applications
- Use DRM/KMS for overlay functionality

---

### 4.6 Video Output Overlay Interface
**Status**: 🔴 Out of Scope

- Deprecated, replaced by DRM/KMS

---

### 4.7 Codec Interface
**Status**: ❌ Not Started

- [ ] Stateful codec interface
- [ ] Encoder setup
- [ ] Decoder setup
- [ ] Codec state management
- [ ] `VIDIOC_ENCODER_CMD` / `VIDIOC_DECODER_CMD`
- [ ] Drain/flush operations
- [ ] Dynamic resolution change

**Priority**: High (hardware codec support)

**Deliverables**:
- Create `v4l2/codec.go`
- Implement encoder/decoder state machines
- Add codec command support
- Create H.264 encoder example
- Create H.264 decoder example

---

### 4.8 Effect Devices Interface
**Status**: 🔴 Out of Scope

- Rarely implemented in modern drivers

---

### 4.9 Raw VBI Data Interface
**Status**: 🔴 Out of Scope

- Legacy analog TV feature
- Teletext/closed captions (use modern subtitle standards instead)

---

### 4.10 Sliced VBI Data Interface
**Status**: 🔴 Out of Scope

- Legacy analog TV feature

---

### 4.11 Teletext Interface
**Status**: 🔴 Out of Scope

- Legacy analog TV feature

---

### 4.12 Radio Interface
**Status**: ❌ Not Started

- [ ] FM/AM radio tuner support
- [ ] RDS/RBDS data
- [ ] Radio frequency control
- [ ] Signal strength monitoring

**Priority**: Low (niche hardware)

---

### 4.13 RDS Interface
**Status**: ❌ Not Started

- [ ] RDS data structures
- [ ] Program information
- [ ] Radio text

**Priority**: Low

---

### 4.14 Software Defined Radio Interface (SDR)
**Status**: ❌ Not Started

- [ ] `V4L2_BUF_TYPE_SDR_CAPTURE`
- [ ] IQ sample formats
- [ ] Sample rate control
- [ ] Tuning and gain control

**Priority**: Low (specialized hardware)

---

### 4.15 Touch Devices
**Status**: 🔴 Out of Scope

- Touchscreen data should use input subsystem, not V4L2

---

### 4.16 Media Controller
**Status**: ❌ Not Started

- [ ] `MEDIA_IOC_DEVICE_INFO`
- [ ] `MEDIA_IOC_ENUM_ENTITIES`
- [ ] `MEDIA_IOC_ENUM_LINKS`
- [ ] `MEDIA_IOC_SETUP_LINK`
- [ ] Pipeline configuration
- [ ] Subdevice management
- [ ] Pad-level configuration

**Priority**: High (required for CSI-2 cameras, complex pipelines)

**Deliverables**:
- Create `media/` package for Media Controller API
- Implement entity/link enumeration
- Add pipeline setup
- Create Raspberry Pi Camera Module example
- Create MIPI CSI-2 camera example

---

### 4.17 Sub-device Interface
**Status**: ❌ Not Started

- [ ] `VIDIOC_SUBDEV_ENUM_MBUS_CODE`
- [ ] `VIDIOC_SUBDEV_ENUM_FRAME_SIZE`
- [ ] `VIDIOC_SUBDEV_G_FMT` / `VIDIOC_S_FMT`
- [ ] Pad-level format negotiation
- [ ] Subdevice controls

**Priority**: Medium (needed with Media Controller)

**Deliverables**:
- Add subdevice format handling
- Create subdevice control API
- Integrate with Media Controller

---

## 5. Event Handling

### 5.1 Event Interface
**Status**: ❌ Not Started

- [ ] `VIDIOC_SUBSCRIBE_EVENT`
- [ ] `VIDIOC_UNSUBSCRIBE_EVENT`
- [ ] `VIDIOC_DQEVENT`
- [ ] Event types:
  - [ ] `V4L2_EVENT_VSYNC` - Vertical sync events
  - [ ] `V4L2_EVENT_EOS` - End of stream
  - [ ] `V4L2_EVENT_CTRL` - Control changes
  - [ ] `V4L2_EVENT_FRAME_SYNC` - Frame sync events
  - [ ] `V4L2_EVENT_SOURCE_CHANGE` - Source change (resolution, etc.)
  - [ ] `V4L2_EVENT_MOTION_DET` - Motion detection

**Priority**: Medium (useful for codec state changes, resolution changes)

**Deliverables**:
- Create `v4l2/events.go`
- Implement event subscription
- Add event polling/dequeueing
- Create event monitoring example

---

## 6. Memory-to-Memory Interface

### 6.1 Memory-to-Memory
**Status**: ❌ Not Started

- [ ] M2M device detection
- [ ] Dual queue management (output + capture)
- [ ] Job scheduling
- [ ] Image processing devices
- [ ] Video conversion devices
- [ ] Codec devices (encoder/decoder)

**Priority**: High (hardware codecs, scalers, converters)

**Deliverables**:
- Create M2M device abstraction
- Implement dual-queue management
- Create image scaler example
- Create format converter example

---

## 7. Selection API (Cropping, Composing, Scaling)

### 7.1 Crop/Selection API
**Status**: 🚧 Partial

- [x] `VIDIOC_CROPCAP` - Query crop capabilities (basic)
- [ ] `VIDIOC_G_CROP` - Get crop rectangle
- [ ] `VIDIOC_S_CROP` - Set crop rectangle
- [ ] `VIDIOC_G_SELECTION` - Get selection rectangle
- [ ] `VIDIOC_S_SELECTION` - Set selection rectangle
- [ ] Selection targets (crop, compose, crop bounds, etc.)
- [ ] Scaling support

**Priority**: Medium (useful for region of interest, scaling)

**Deliverables**:
- Complete crop/selection API
- Add selection target support
- Create crop example
- Create compose/scale example

---

## 8. MPEG Compression

### 8.1 MPEG Compression API
**Status**: 🔴 Deprecated

- Replaced by extended controls (codec controls)
- See section 1.15 Codec Control Reference

---

## 9. Memory Management

### 9.1 CREATE_BUFS
**Status**: ❌ Not Started

- [ ] `VIDIOC_CREATE_BUFS` - Create buffers
- [ ] Dynamic buffer allocation
- [ ] Per-buffer format configuration

**Priority**: Low (optional optimization)

**Deliverables**:
- Add CREATE_BUFS support
- Add dynamic buffer allocation
- Benchmark vs REQBUFS

---

### 9.2 DMABUF Exporting
**Status**: ❌ Not Started

- [ ] `VIDIOC_EXPBUF` - Export buffer as DMA-BUF
- [ ] DMA-BUF fd management

**Priority**: Medium (needed for GPU integration)

---

### 9.3 DMABUF Importing
**Status**: ❌ Not Started

- [ ] Import DMA-BUF from other subsystems
- [ ] GPU-allocated buffer import
- [ ] Display buffer sharing

**Priority**: Medium

---

## 10. Debugging and Tracing

### 10.1 Debug Interfaces
**Status**: ❌ Not Started

- [ ] `VIDIOC_LOG_STATUS` - Log device status
- [ ] Debug register access (if supported)

**Priority**: Low

---

## Summary Statistics

### By Status
- ✅ Complete: ~15 items
- 🚧 Partial: ~8 items
- ❌ Not Started: ~45 items
- 🔴 Out of Scope: ~8 items

### By Priority
- **High**: Codec interface, M2M, Media Controller, Codec controls, DMA-BUF
- **Medium**: Multi-planar, Video output, DV timings, Extended controls
- **Low**: Priority API, Read/write I/O, Legacy features

---

## Implementation Phases

### Phase 1: High-Priority Core Features (Current Focus)
1. Frame Statistics API ✅
2. Codec Control Reference
3. Memory-to-Memory Interface
4. Media Controller API
5. DMA-BUF Support (Export/Import)

### Phase 2: Professional Video Features
1. Multi-planar support
2. DV Timings
3. Event Interface
4. Selection API (Crop/Compose)

### Phase 3: Additional Device Types
1. Video Output Interface
2. Audio Inputs/Outputs
3. Codec State Management

### Phase 4: Specialized Features
1. User Pointer I/O
2. Async I/O
3. Tuners/Modulators
4. SDR Interface

---

## Notes

- Items marked 🔴 Out of Scope are legacy/deprecated features not recommended for new applications
- Priority ratings consider:
  - User demand
  - Hardware availability
  - Complexity
  - Dependencies
- Each roadmap item should have:
  - Clear deliverables
  - Test coverage
  - Documentation
  - Example code

---

**Last Updated**: 2025-10-19
**Based On**: Linux Kernel Documentation v6.6+
