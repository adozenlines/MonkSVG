MonkVG -- An OpenVG implementation
==================================

## Warning

__This version is a special version containing only the iOS Branch, was modified to split into its component projects and optimized to use the latest tool versions (as well as the latest work pulls from other branches)! Please go to http://github.com/micahpearlman/MonkVG for the much safer official version!__

## Overview

MonkVG is an OpenVG 1.1 *like* vector graphics API implementation currently using an OpenGL ES (1.1 or 2.0) backend that should be compatible with any HW that supports OpenGL ES 1.1 which includes most iOS and Android devices as well as OSX and Windows platforms. 

This is an open source BSD licensed project that is in active development. Contributors and sponsors welcome.

It can be found at GitHub http://github.com/micahpearlman/MonkVG

## Whats New

- (10/13/2016) iOS only. Projects are now split up and updated to their latest version.
- (1/22/2012) Now supports OpenGL ES 1.1 *AND* 2.0

## Installation

Use git to clone:
- Official version: <tt>$ git clone git@github.com:micahpearlman/MonkVG.git</tt>
- Special iOS only version: <tt>$ git clone git@github.com:sakamura/OpenVG.git</tt>

## What is implemented

- Most all path segment commands including: moves, lines, bezier curves, elliptical arcs.
- Robust contour tesselation supporting both fill rules.
- Very basic stroking.
- Most paints including: Solid color fill, linear and radial gradients.  
- Bitmap font rendering.
- Bitmap image rendering.
- OpenVG utility library (see vgu.h)
	
## TODO
- Pattern fills and strokes.
- Gradient fills on strokes (works for fills).
- Stroke font rendering (may work just untested).
- Various blending modes (somewhat working already).
- Scissoring and masking.
- Improve stroking geometry generation.
- Batch optimization seems to be broken.
- Remove OpenGL ES 1 support.
- Support OgenGL ES 2 & 3 natively.
- Major cleanup to optimize usage for OpenGL only.
- Triangle occlusion culling & other runtime optimizations.

## Probably never support
- Image filters.
- Anti-aliasing. (Though this can be supported outside OpenVG.  For example iOS fullscreen AA glRenderbufferStorageMultisampleAPPLE method)
- Metal optimization.

## Extensions

MonkVG was originally created for games, so speed has usually been prefered over quality or full OpenVG compliance.  To improve speed there have been MonkVG specific extensions. See "vgext.h" for some of the details.

## Contributors

- Paul Holden (Initial Android Port)  
- Vincent Richomme (Windows Port)  
- Gav Wood (Android and Linux Port) 
- Michel Donais (iOS updated version), includes multiple pull requests (See graphs)

Also Luke contributed a great article on how to integrate MonkVG + MonkSWF with Cocos2D: http://blog.zincroe.com/2011/11/displaying-a-swf-on-the-iphone-with-cocos2d-and-monkswf/

## Simple Example

NOTE:  MonkVG will not create a OpenGL context, it is the applications responsibility to create there own OpenGL context.
Also, if your application does any other OpenGL rendering it should save off the GL state and then restore before calling any MonkVG methods.

<tt>

	VGPaint _paint;
	VGPath _path;
	void init() {
		... setup platform specific opengl ...
		// setup the OpenVG context
		vgCreateContextMNK( 320, 480, VG_RENDERING_BACKEND_TYPE_OPENGLES20 );

		...OR... for OpenGL ES 1.1

		vgCreateContextMNK( 320, 480, VG_RENDERING_BACKEND_TYPE_OPENGLES11 );

		// create a paint
		_paint = vgCreatePaint();
		vgSetPaint(_paint, VG_FILL_PATH );
		VGfloat color[4] = { 1.0f, 0.0f, 0.0f, 1.0f };
		vgSetParameterfv(_paint, VG_PAINT_COLOR, 4, &color[0]);

		// create a box path
		_path = vgCreatePath(VG_PATH_FORMAT_STANDARD, VG_PATH_DATATYPE_F,1,0,0,0, VG_PATH_CAPABILITY_ALL);
		vguRect( _path, 50.0f, 50.0f, 90.0f, 50.0f );
	}

	void draw() {
		... save any GL state here ...
		... start opengl context ...

		/// draw the basic path
		vgSeti(VG_MATRIX_MODE, VG_MATRIX_PATH_USER_TO_SURFACE);
		vgLoadIdentity();
		vgTranslate( screenWidth/2, screenHeight/2 );
		vgSetPaint( _paint, VG_FILL_PATH );
		vgDrawPath( _path, VG_FILL_PATH );
		
		... end opengl context ...
		... restore any GL state here ...
	}
</tt>

###Build steps

Open XCode library project. Profit.
