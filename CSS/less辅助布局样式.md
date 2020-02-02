# less辅助布局样式

```less
// 全局设置
body {
	font-family: 'Helvetica Neue', Helvetica, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', '微软雅黑', Arial, sans-serif;
}

// 辅助布局循环
@directionList: t, b, l, r, true;
@realDirectionList: top, bottom, left, right;
@sizeList: xs, sm, md, lg, xl;

.loopDirectionTools(@prop,@i:1) when (@i < length(@directionList) + 1) {
	@direction: extract(@directionList, @i);
	.loopAll() when(@direction) {
		.loopSize(@j:1) when (@j < length(@sizeList) + 1) {
			@size: extract(@sizeList, @j);

			.c-@{prop}-@{size} {
				@{prop}: 4px * @j;
			}
			.loopSize(@j + 1);
		}
		.loopSize(1);
	}

	.loop() when not (@direction) {
		@realDirection: extract(@realDirectionList, @i);
		.loopSize(@j:1) when (@j < length(@sizeList) + 1) {
			@size: extract(@sizeList, @j);
			.c-@{prop}-@{direction}-@{size} {
				@{prop}-@{realDirection}: 4px * @j;
			}
			.loopSize(@j + 1);
		}
		.loopSize(1);
	}

	.loopAll();
	.loop();
	.loopDirectionTools(@prop, @i + 1);
}

@extendDirectionList: tb, lr;

.loopExtendDirectionTools(@prop,@i:1) when (@i<length(@extendDirectionList) + 1) {
	@extendDirection: extract(@extendDirectionList, @i);
	.loopSize(@j:1) when (@j < length(@sizeList) + 1) {
		@size: extract(@sizeList, @j);
		.c-@{prop}-@{extendDirection}-@{size} {
			@direction1: extract(@realDirectionList, 2 * @i - 1);
			@direction2: extract(@realDirectionList, 2 * @i);
			@{prop}-@{direction1}: 4px * @j;
			@{prop}-@{direction2}: 4px * @j;
		}
		.loopSize(@j + 1);
	}
	.loopSize(1);
	.loopExtendDirectionTools(@prop, @i + 1);
}

@loopDirectionOptions: margin, padding;
.loopDirection(@i:1) when (@i<length(@loopDirectionOptions) + 1) {
	@option: extract(@loopDirectionOptions, @i);
	.loopDirectionTools(@option, 1);
	.loopExtendDirectionTools(@option, 1);
	.loopDirection(@i+1);
}

.loopDirection(1);

// 盒子大小设置
@boxSizeList: w, h;
@realBoxSizeList: width, height;
@scaleList: 0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1;

.loopBoxSizeTools(@i:1) when(@i < length(@boxSizeList) + 1) {
	@prop: extract(@boxSizeList, @i);
	@realProp: extract(@realBoxSizeList, @i);

	.loopSize(@j:1) when(@j < length(@scaleList) + 1) {
		@size: extract(@scaleList, @j);
		@sizeName: @size * 100;
		.c-@{prop}-@{sizeName} {
			@{realProp}: percentage(@size);
		}
		.loopSize(@j + 1);
	}
	.loopSize(1);
	.loopBoxSizeTools(@i + 1);
}

.loopBoxSizeTools(1);

// flex布局
@flex-direction: row, row-reverse, column, column-reverse;
@flex-wrap: nowrap, wrap, wrap-reverse;

@justify-content: flex-end, center, space-between, space-around;
@align-items: flex-end, center, baseline, stretch;
@align-content: flex-start, flex-end, center, space-between, space-around, stretch;
@align-self: auto, flex-start, flex-end, center, baseline, stretch;

@flex-grow: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12;
@flex-shrink: 0;
@flex-basis: auto;

@flexPropOptions: justify-content, align-items, align-content, align-self, flex-direction, flex-wrap, flex-grow, flex-shrink, flex-basis;
@flexLayoutName: jc, ai, ac, as;

.loopFlexTools(@flexPropOptions,@i:1) when(@i < length(@flexPropOptions) + 1) {
	@flexPropOption: extract(@flexPropOptions, @i);

	.loopFlexLayout(@prop,@j:1) when (@j<length(@prop) + 1) and (@i <= 4) {
		@layoutName: extract(@flexLayoutName, @i);
		@propItem: extract(@prop, @j);
		.c-@{layoutName}-@{propItem} {
			@{flexPropOption}: @propItem;
		}
		.loopFlexLayout(@prop, @j + 1);
	}

	.loopFlexExtend(@prop, @j:1) when (@j < length(@prop) + 1) and(@i > 4) {
		@propItem: extract(@prop, @j);

		.loopFlexGrow() when (@prop = @flex-grow) {
			.c-flex-@{propItem} {
				@{flexPropOption}: @propItem;
			}
		}

		.loopFlexGrow();

		.loopFlexNormal() when not (@prop = @flex-grow) {
			.c-@{flexPropOption}-@{propItem} {
				@{flexPropOption}: @propItem;
			}
		}
		.loopFlexNormal();

		.loopFlexExtend(@prop, @j + 1);
	}

	.loopFlexLayout(@@flexPropOption, 1);
	.loopFlexExtend(@@flexPropOption, 1);

	.loopFlexTools(@flexPropOptions, @i + 1);
}

.c-flex {
	display: flex;
}

.loopFlexTools(@flexPropOptions, 1);

// 颜色相关

@red: #e54d42;
@orange: #f37b1d;
@yellow: #fbbd08;
@olive: #8dc63f;
@green: #39b54a;
@cyan: #1cbbb4;
@blue: #0081ff;
@purple: #6739b6;
@mauve: #9c26b0;
@pink: #e03997;
@brown: #a5673f;
@grey: #8799a3;
@gray: #f0f0f0;
@black: #333333;
@white: #ffffff;

@colorList: red, orange, yellow, olive, green, cyan, blue, purple, mauve, pink, brown, grey, gray, black, white;

@colorPropOptions: border-color, background-color, color;
@border-color: border;
@background-color: bg;
@color: text;

.loopColorTools(@colorPropOptions,@i:1) when (@i < length(@colorPropOptions) + 1) {
	@colorPropOption: extract(@colorPropOptions, @i);
	.loopColor(@j:1) when (@j < length(@colorList) + 1) {
		@colorItem: extract(@colorList, @j);
		@mapPropName: @@colorPropOption;
		.loopBorder() when (@i = 1) {
			.c-@{mapPropName}-@{colorItem} {
				border:1px solid;
				@{colorPropOption}: @@colorItem;
			}
		}
		.loopNormal() when not (@i = 1) {
			.c-@{mapPropName}-@{colorItem} {
				@{colorPropOption}: @@colorItem;
			}
		}
		.loopBorder();
		.loopNormal();
		.loopColor(@j + 1);
	}

	.loopColor(1);
	.loopColorTools(@colorPropOptions, @i + 1);
}

.loopColorTools(@colorPropOptions, 1);

// 文本
.c-text-bold {
	font-weight: bold;
}
.c-text-left {
	text-align: left;
}
.c-text-right {
	text-align: right;
}
.c-text-center {
	text-align: center;
}

.c-text-content {
	line-height: 1.6;
}

.c-text-cut {
	text-overflow: ellipsis;
	white-space: nowrap;
	overflow: hidden;
}

.text-Abc {
	text-transform: Capitalize;
}

.text-ABC {
	text-transform: Uppercase;
}

.text-abc {
	text-transform: Lowercase;
}

.loopFontSizeTools(@i:1) when(@i < length(@sizeList) + 1) {
	@size: extract(@sizeList, @i);
	.c-text-@{size} {
		font-size: 10px + (@i - 1) * 2;
	}
	.loopFontSizeTools(@i + 1);
}

.loopFontSizeTools(1);

.c-text-xxl {
	font-size: 22px;
}

.c-text-sl {
	font-size: 40px;
}

.c-text-xsl {
	font-size: 60px;
}
```

