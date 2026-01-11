/**
 * 智能JSON解析方法
 * @param {any} data - 需要解析的数据
 * @returns {any} 解析后的数据
 */
export function safeJsonParse(data) {
  // 如果不是字符串，直接返回
  if (typeof data !== 'string') {
    return data
  }

  // 如果是空字符串，返回null
  if (!data.trim()) {
    return null
  }

  try {
    const parsed = JSON.parse(data)

    // 如果解析结果是数组，需要递归检查数组中的每个元素
    if (Array.isArray(parsed)) {
      return parsed.map(item => safeJsonParse(item))
    }

    // 如果解析结果是对象，需要递归检查对象中的每个值
    if (parsed && typeof parsed === 'object') {
      const result = {}
      for (const key in parsed) {
        if (parsed.hasOwnProperty(key)) {
          result[key] = safeJsonParse(parsed[key])
        }
      }
      return result
    }

    // 其他情况直接返回解析结果
    return parsed
  } catch (error) {
    // 解析失败，返回原始数据
    console.warn('JSON解析失败，返回原始数据:', data)
    return data
  }
}