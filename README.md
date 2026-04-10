class ChatMimicApp:
    """主应用入口"""
    
    def __init__(self, api_key: str, target_name: str):
        self.parser = ChatDataParser(target_name)
        self.extractor = PersonaExtractor()
        self.prompt_builder = PersonaPromptBuilder()
        self.engine = MimicChatEngine(api_key)
        self.target_name = target_name
    
    def setup(self, file_path: str, 
              start_date: str, end_date: str,
              platform: str = "wechat"):
        
        print("📂 解析聊天数据...")
        if platform == "wechat":
            df = self.parser.parse_wechat(file_path, start_date, end_date)
        
        print(f"✅ 共找到 {len(df[df['is_target']])} 条目标消息")
        
        print("🔍 分析人物特征...")
        persona = self.extractor.extract_all_features(df)
        
        print("📝 提取对话样本...")
        samples = self.prompt_builder.extract_dialogue_samples(df)
        
        print("🤖 构建AI人格...")
        system_prompt = self.prompt_builder.build_system_prompt(
            persona, samples, self.target_name
        )
        
        self.engine.initialize(system_prompt)
        self.sends_multiple = persona['conversation_rhythm']['sends_multiple']
        
        print(f"✨ 准备完成！现在可以和模拟的{self.target_name}对话了\n")
        return persona
    
    def run_interactive(self):
        print(f"━━━ 开始与 {self.target_name} 对话 (输入 quit 退出) ━━━\n")
        
        while True:
            user_input = input("你: ").strip()
            if user_input.lower() == 'quit':
                break
            if not user_input:
                continue
            
            replies = self.engine.simulate_multi_message(
                user_input, self.sends_multiple
            )
            
            import time
            for reply in replies:
                time.sleep(0.5)  # 模拟打字延迟
                print(f"{self.target_name}: {reply}")


# 使用示例
if __name__ == "__main__":
    app = ChatMimicApp(
        api_key="your-api-key",
        target_name="小明"
    )
    
    persona = app.setup(
        file_path="wechat_chat.txt",
        start_date="2023-01-01",
        end_date="2024-01-01",
        platform="wechat"
    )
    
    # 显示分析结果
    print("📊 人物特征分析:")
    print(f"  语言风格: {persona['length_preference']['style']}")
    print(f"  情感倾向: {persona['emotional_tone']}")
    print(f"  口头禅: {list(persona['speech_patterns']['fillers'].keys())[:5]}")
    
    app.run_interactive()
