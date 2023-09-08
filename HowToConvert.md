# ワールド空間、スクリーン空間、ビューポート空間の位置の変換
<br>

## ワールド空間について
ワールド空間とは、シーンの3D空間のことです。
<br>
ワールド空間の (x, y, z) = (0, 0, 0) を原点とする座標をワールド座標と呼びます。
<br>

### ローカル座標
また、親を持つ子ゲームオブジェクトの Transform コンポーネントで指定できる値は、親に対する相対的な値になります。
親ゲームオブジェクトの位置を原点とする座標をローカル座標と呼びます。
<br>

## スクリーン空間について
スクリーン空間はカメラで表示している領域をスクリーン座標で表したものです。
<br>
スクリーン座標の値は解像度と一致しています。ピクセルが単位です。
<br>
<br>
スクリーン座標は左下 (0, 0) 、右上 (pixelWidth, pixelHeight) です。
<br>
タッチ位置の座標やマウスの座標はスクリーン座標で取得します。
<br>

## ビューポート空間について
ビューポート空間はカメラで表示している領域をビューポート座標で表したものです。
<br>
ビューポート空間のスクリーン座標の値を0～1の間で正規化した座標をビューポート座標と呼びます。
<br>
<br>
ビューポート座標は左下を (x, y, z) = (0,0,1) として、右上 (x, y, z) = (1,1,1) としたものです。
<br>
時計回りに左下 (0, 0) 、右下 (1, 0) 、右上 (1, 1) 、左上 (0, 1) となります。それらの間は0〜1までの小数点で表されます。
<br>
スクリーン座標の値を0～1の間で正規化しているため、様々な解像度における利用を想定していて、左上や右下の位置を指定する際などに用います。
<br>
<br>

## ワールド空間、スクリーン空間、ビューポート空間の位置の変換について
変換するためのスクリプトは以下です。
<br>
### ワールド座標からビューポート座標、スクリーン座標に変換する。

```c#
public class TransformCoordinates : MonoBehaviour {

	// 変換するワールド座標
	public Transform targetWorldPos;

	// カメラ
	public Camera camera;

	// ワールド座標を変換したスクリーン座標
	Vector3 screenPos;
	// ワールド座標を変換したビューポート座標
	Vector3 viewportPos;

	void Update() {
		// ワールド空間の position をスクリーン空間に変換します。
		screenPos = camera.WorldToScreenPoint(targetWorldPos.position);

		// ワールド空間の position をビューポート空間に変換します。
		viewportPos = cam.WorldToViewportPoint(targetWorldPos.position);
	}
}
```

<br>
### スクリーン座標からワールド座標、ビューポート座標に変換する。

```c#
public class TransformCoordinates : MonoBehaviour {

	// 変換するスクリーン座標(0~解像度)
	public Vector3 targetScreenPos;

	// カメラ
	public Camera camera;

	// スクリーン座標を変換したワールド座標
	Vector3 worldPos;
	// スクリーン座標を変換したビューポート座標
	Vector3 viewportPos;

	void Update() {
		// スクリーン空間の position をワールド空間に変換します。
		worldPos = cam.ViewportToWorldPoint(targetScreenPos);
  
  		// スクリーン空間の position をビューポート空間に変換します。
		viewportPos = camera.ViewportToScreenPoint(targetScreenPos);
	}
}
```

<br>
### ビューポート座標からワールド座標、スクリーン座標に変換する。

```c#
public class TransformCoordinates : MonoBehaviour {

	// 変換するビューポート座標(0~1)
	public Vector3 targetViewportPos;

	// カメラ
	public Camera camera;

	// ビューポート座標を変換したワールド座標
	Vector3 worldPos;
	// ビューポート座標を変換したスクリーン座標
	Vector3 screenPos;

	void Update() {
		// ビューポート空間の position をワールド空間に変換します。
		worldPos = cam.ViewportToWorldPoint(targetViewportPos);
  
  		// ビューポート空間の position をスクリーン空間に変換します。
		screenPos = camera.ViewportToScreenPoint(targetViewportPos);
	}
}
```
