=======================
 Defining prompts via hooks 
=======================


The following example shows how to define input and output prompts via hooks::

    #!python
    import os
    from IPython import ipapi, Prompts
    
    def myinputprompt(self, cont):
        ip = self.api
        count = str(len(ip.user_ns["_ih"]))
        colors = Prompts.PromptColors[""].colors
        pwd = os.getcwd()
        if cont:
            return "%s%s%s: " % ( \
                colors.in_prompt2,
                "."*(4+len(pwd)+1+len(count)+1),
                colors.normal)
        else:
                return "%sIn [%s|%s%s%s]: %s" % ( \
                colors.in_prompt,
                pwd,
                colors.out_number,
                count,
                colors.in_prompt,
                colors.normal)
    
    def myoutputprompt(self):
        ip = self.api
        count = str(len(ip.user_ns["_ih"]))
        colors = Prompts.PromptColors[""].colors
        pwd = os.getcwd()
        prompt = "%sOut[%s|%s%s%s]: %s" % ( \
            colors.out_prompt,
            pwd,
            colors.out_number,
            count,
            colors.out_prompt,
            colors.normal)
        return prompt
    
    ipapi.get().set_hook("generate_prompt", myinputprompt)
    ipapi.get().set_hook("generate_output_prompt", myoutputprompt)


