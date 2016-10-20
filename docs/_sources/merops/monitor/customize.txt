自定义Formatter
===============

自定义Formatter只需要实现IFormatter接口。

IFormatter.Name是用于在客户端选择使用哪个已添加的Formatter，在服务中不会使用到。

客户端调用服务时会传把当前Formatter.SupportMimes中的第一个元素传到服务端，服务端会在Formatter中找到第一个支持这个mime的Formatter用于反序列化参数及序列化返回结果。

::

	public interface IFormatter
	{
		/// <summary>
		/// format name, case sensitive, suggest use lower case, eg: xml, json
		/// </summary>
		string Name { get; }

		/// <summary>
		/// same as HTTP Content-Type, such as application/xml, application/json
		/// </summary>
		List<string> SupportMimes { get; }

		/// <summary>
		/// get if the Formatter can serialize Type of Exception
		/// </summary>
		bool SupportException { get; }

		/// <summary>
		/// 
		/// </summary>
		/// <param name="inputStream"></param>
		/// <param name="targetType"></param>
		/// <returns></returns>
		object Deserialize(Stream inputStream, Type targetType);

		/// <summary>
		/// 
		/// </summary>
		/// <param name="reader"></param>
		/// <param name="targetType"></param>
		/// <returns></returns>
		object Deserialize(TextReader reader, Type targetType);

		/// <summary>
		/// 
		/// </summary>
		/// <param name="outputStream"></param>
		/// <param name="source"></param>
		/// <param name="type"></param>
		void Serialize(Stream outputStream, object source, Type type);

		/// <summary>
		/// 
		/// </summary>
		/// <param name="writer"></param>
		/// <param name="source"></param>
		/// <param name="type"></param>
		void Serialize(TextWriter writer, object source, Type type);
	}