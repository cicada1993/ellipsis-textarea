<!-- inspired by https://stackoverflow.com/questions/4719777/finding-line-breaks-in-textarea-that-is-word-wrapping-arabic-text/4722395#4722395 -->
<template>
  <div class="ellipsis-textarea">
    <textarea
      ref="ta"
      wrap="off"
      :rows="rows"
      readonly
      v-model="fitText"
    ></textarea>
  </div>
</template>

<script>
export default {
  name: "ellipsis-textarea",
  props: {
    maxRows: {
      // 最大显示行数
      type: Number,
      default: 1,
    },
    text: {
      // 文本内容
      type: String,
      default: "",
    },
  },
  watch: {
    text(val, oldVal) {
      this.applyEllipsis();
    },
  },
  data() {
    return {
      fitText: "",
      rows: 1,
      textLines: [],
    };
  },
  methods: {
    applyEllipsis() {
      this.rows = 1;
      let _ta = this.$refs.ta;
      let strRawValue = this.text; // 读取文本内容
      _ta.value = "";
      _ta.setAttribute("wrap", "off");
      let emptyWidth = _ta.scrollWidth; // 空时宽度
      const testBreak = (strTest) => {
        // 检测行超出 如果填入文本后 超出宽度表示要换行了
        _ta.value = strTest;
        return _ta.scrollWidth > emptyWidth;
      };

      /**
       * 二分法查找换行位置
       */
      const findNextBreakLength = (strSource, nLeft, nRight) => {
        let nCurrent;
        if (typeof nLeft == "undefined") {
          // 首次查找 给定默认位置
          nLeft = 0;
          nRight = -1;
          nCurrent = 64; // 给定一个右侧初始位置
        } else {
          if (nRight == -1) nCurrent = nLeft * 2;
          // 偏小嫌疑分支
          else if (nRight - nLeft <= 1) return Math.max(2, nRight);
          // 命中了
          else nCurrent = nLeft + (nRight - nLeft) / 2; // 偏大嫌疑分支
        }
        let strTest = strSource.substr(0, nCurrent); // 截取从0~
        let bLonger = testBreak(strTest);
        if (bLonger) nRight = nCurrent;
        // nCurrent下 检测到超出  更新右边界（测试范围偏大嫌疑）
        else {
          if (nCurrent >= strSource.length) return null; // 永远不会超出
          nLeft = nCurrent; // nCurrent下  未检测到超出 更新左边界（测试范围偏小嫌疑）
        }
        return findNextBreakLength(strSource, nLeft, nRight);
      };

      let i = 0,
        j;
      let strNewValue = "";
      while (i < strRawValue.length) {
        let breakOffset = findNextBreakLength(strRawValue.substr(i));
        if (breakOffset === null) {
          strNewValue += strRawValue.substr(i);
          break;
        }
        let nLineLength = breakOffset - 1;
        for (j = nLineLength - 1; j >= 0; j--) {
          let curChar = strRawValue.charAt(i + j);
          if (curChar == " " || curChar == "-" || curChar == "+") {
            nLineLength = j + 1;
            break;
          }
        }
        strNewValue += strRawValue.substr(i, nLineLength) + "\n";
        i += nLineLength;
      }
      this.textLines = strNewValue.split("\n");
      if (this.textLines.length > this.maxRows) {
        // 超出限定行数 需要在要显示的最后一行尾部添加...
        let preLines = this.textLines.slice(0, this.maxRows - 1);
        let lastText = this.textLines[this.maxRows - 1];
        let ellipsisIndex = lastText.length - 1;
        let ellipsis = "...";
        while (testBreak(lastText.substr(0, ellipsisIndex) + ellipsis)) {
          ellipsisIndex--;
        }
        lastText = lastText.substr(0, ellipsisIndex) + ellipsis;
        preLines.push(lastText);
        this.rows = this.maxRows;
        _ta.setAttribute("wrap", "");
        this.fitText = preLines.join("");
      } else {
        this.rows = this.textLines.length;
        _ta.setAttribute("wrap", "");
        this.fitText = this.text;
      }
    },
  },
  created() {},
  mounted() {
    this.$nextTick(() => {
      this.applyEllipsis();
    });
  },
};
</script>

<style  scoped>
.ellipsis-textarea textarea {
  width: 100%;
  resize: none;
  border: none;
  padding: 0px;
}
</style>